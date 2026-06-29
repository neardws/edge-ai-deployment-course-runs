# 2026-06-29 Jetson Login Check

## Goal

Act as a student starting the Jetson chapter and verify that the edge device can be reached before running environment commands.

This record intentionally omits the device address, usernames, host key, and any private network details.

## Result

| Check | Result |
| --- | --- |
| SSH service reachable | Yes |
| Host key can be accepted in a temporary known-hosts file | Yes |
| Login with the provided account | Failed |
| Login with common lab fallback accounts | Failed |
| Jetson environment snapshot | Not reached |

Observed failure class:

```text
Permission denied (publickey,password)
```

One common default account also returned:

```text
Permission denied (publickey)
```

Student judgment: the course should add a login preflight before `mkdir -p ~/edge-ai-lab`, because the current Jetson lab assumes terminal access is already solved.

## Commands

```bash
ssh -o BatchMode=yes -o ConnectTimeout=8 \
  <jetson-user>@<jetson-host> \
  'echo SSH_OK; hostname; whoami'
```

If host key verification fails after a device reimage or IP reuse, confirm the new fingerprint with the instructor before changing local SSH records.

## Course Feedback

1. Add a Step 0 login preflight to the Jetson lab.
2. Separate SSH host-key problems from account/key/password problems.
3. Tell students to stop before Step 1 if `Permission denied` appears.
4. Ask instructors to provide one confirmed login method before assigning the Jetson lab.
