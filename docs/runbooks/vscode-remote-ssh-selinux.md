# VS Code Remote-SSH blocked by SELinux

## Purpose

This runbook describes how to diagnose VS Code Remote-SSH failures where
regular SSH authentication succeeds but SSH port forwarding to the VS Code
Server fails on a Fedora host running SELinux.

## Symptoms

Regular SSH works:

    ssh fedora-lab

VS Code Remote-SSH fails with messages such as:

    channel 2: open failed: connect failed: open failed

The VS Code Server may successfully start and report a local listening port:

    listeningOn==41797==

but the SSH tunnel cannot connect to that port.

## Diagnosis

### 1. Verify SSH connectivity

From the client:

    ssh fedora-lab "echo OK && uname -a && id"

If this succeeds, basic SSH connectivity and authentication are working.

### 2. Verify SSH forwarding configuration

On the Fedora host:

    sudo sshd -T | grep -Ei \
      'allowtcpforwarding|disableforwarding|permitopen|permitlisten'

Expected:

    allowtcpforwarding yes
    disableforwarding no
    permitopen any
    permitlisten any

### 3. Check SELinux

    getenforce

If the result is:

    Enforcing

inspect recent AVC denials:

    sudo ausearch -m AVC,USER_AVC -ts recent

Look for a denial similar to:

    avc: denied { name_connect }
    comm="sshd-session"
    scontext=system_u:system_r:sshd_session_t:s0-s0:c0.c1023
    tcontext=system_u:object_r:ephemeral_port_t:s0
    tclass=tcp_socket

This indicates that SELinux is preventing the SSH session from connecting to
the ephemeral port used by the VS Code Server.

## Diagnostic confirmation

Temporarily switch SELinux to permissive mode:

    sudo setenforce 0

Retry the Remote-SSH connection.

If it succeeds, SELinux is confirmed as the blocking layer.

Immediately restore enforcing mode after the test:

    sudo setenforce 1

Never use permissive mode as the permanent solution.

## Resolution

Install the SELinux policy development utilities if required:

    sudo dnf install -y policycoreutils-python-utils

Generate a policy from the relevant AVC denial:

    sudo ausearch -c 'sshd-session' -m AVC -ts recent \
      | audit2allow -M vscode-ssh

Review the generated policy before installation:

    cat vscode-ssh.te

Only install the module after verifying that the generated permissions match
the expected Remote-SSH operation.

Example required permission:

    allow sshd_session_t ephemeral_port_t:tcp_socket name_connect;

Install:

    sudo semodule -i vscode-ssh.pp

Ensure SELinux remains enabled:

    sudo setenforce 1
    getenforce

Expected:

    Enforcing

## Verification

Connect to the host using VS Code Remote-SSH.

Verify the module:

    sudo semodule -l | grep vscode

Expected:

    vscode-ssh

Then check for new AVC denials:

    sudo ausearch -m AVC,USER_AVC -ts recent

A successful connection should not generate a new denial for the operation
covered by the local policy.

## Current VEL Remote-SSH configuration

The VEL workstation currently uses:

    "remote.SSH.useExecServer": false,
    "remote.SSH.enableDynamicForwarding": false

These settings force Remote-SSH to use direct local port forwarding instead
of dynamic SOCKS forwarding while the Fedora/Remote-SSH behavior is being
evaluated.

They should be considered a workaround rather than permanent platform
configuration.

## Rollback

Remove the local SELinux policy:

    sudo semodule -r vscode-ssh

Verify:

    sudo semodule -l | grep vscode

If no output is returned, the module has been removed.

Do not remove the module until Remote-SSH has been tested successfully without
it while SELinux is enforcing.

## Security considerations

Do not solve this issue by permanently disabling SELinux.

Prefer:

1. Identify the AVC denial.
2. Confirm the behavior using permissive mode temporarily.
3. Review the minimum required permission.
4. Install a narrowly scoped local policy.
5. Keep SELinux enforcing.
6. Remove the local policy when it is no longer required.

## Related VEL components

- Fedora host security
- SSH remote administration
- VS Code remote development
- SELinux
- Developer workstation integration

## Follow-up

- Monitor Fedora SELinux policy updates.
- Re-test after relevant Fedora updates.
- Test Remote-SSH with dynamic forwarding enabled.
- Test Remote-SSH with Exec Server enabled.
- Remove the local SELinux module when upstream behavior no longer requires it.
