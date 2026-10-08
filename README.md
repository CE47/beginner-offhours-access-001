# CTIA Guided Lab — Beginner Off-Hours Access and Credential Theft

## The incident

A user account that normally works nine hours a day logged on at two in the morning from a
home address the company had never seen before, then opened twenty-five sensitive files on the
company file server in about nine minutes and sent one archive out of the building. It logged
off fifteen minutes later.

You are the analyst. Work out which account it was, which address it came from, how many files
it read and when, how many bytes actually left, and what you can and cannot prove. Then write it
up.

## What you need

- **Kali Linux**, running inside VirtualBox. Nothing else.
- `git` and `python3`. Both already ship with Kali.

Check them:

```
git --version
python3 --version
```

If either is missing:

```
sudo apt update && sudo apt install -y git python3
```

## No VirtualBox changes. No port forwarding.

**Do not change any VirtualBox network setting and do not set up port forwarding.** The lab
listens on `127.0.0.1` inside the VM only, and that is deliberate: it cannot be reached from your
host or from anywhere else.

This is the first thing students get wrong, so say it twice: **there is no port forwarding in
this lab, and you do not need one.** You will use the browser *inside the VM*.

## Get the lab

```
git clone https://github.com/CE47/beginner-offhours-access-001.git
cd beginner-offhours-access-001
```

Then check you have all the evidence. One command, one number:

```
wc -l *.log | tail -1
```

The total must be exactly **7064**. If it is not, your copy is incomplete — re-clone before you
start.

Every time in this lab is UTC, and every timestamp in the five log files ends in `+00:00`. If a
timestamp in your copy says something else, re-clone.

## Start it

Double-click **`start-lab.desktop`** on the Kali desktop.

If your Kali desktop does not show it, open a terminal in this folder and run:

```
./start-lab.sh
```

Keep that window open the whole time you work. The browser opens by itself. If it does not, the
URL is printed in the terminal window — copy it and paste it into the browser **inside the VM**.

To stop the lab, press `Ctrl+C` in that window.

## The status pill

The pill at the top right of the page tells you what is happening.

| What it says | What it means | What to do |
|---|---|---|
| **Finding the lab…** | the page is looking for a running lab | wait a few seconds |
| **Offline** | no lab server found | start the lab with `start-lab.desktop`, then wait |
| **Real bash connected** | you are connected to a real shell in the evidence folder | carry on |

If it stays on *Offline* while the launcher window is open, press the reload button on the
browser page. The page keeps looking, so you do not have to reload again.

## Working through the steps

- Press **Run** on a command box to run it in the terminal.
- A command with several tools is shown as a **ladder**: one box per stage. Run them from the
  top and watch the output change. The ladder is the lesson, so do not skip to the last box.
- Answer the **checkpoint** question at the bottom of each step in your own words, in the notes
  box. Your teacher reads these.
- **←** and **→** move between steps.
- Press **?** in the top bar for the keyboard shortcuts.

You do not need to understand anything about how these commands work internally. Each step
explains what it wants you to find before it asks you to run anything.

## The surprises

Some commands in this lab return a number that is not what you expect. That is deliberate, and
each one is explained in the step where you meet it. You do not have to discover any of them on
your own.

| What you will notice | Where it is explained |
|---|---|
| Ranking the accounts by **files read** puts `kpatel` first — and that account is innocent | step 7 |
| The account on the ticket read **28** files all day, but only **25** of them are the incident | step 7 |
| **Two** addresses sit outside the company network, and one belongs to a colleague working from home | step 5 |
| On the hour table, the `02` line carries an **ordinary-looking** count. The hour is what is wrong | step 4 |
| There is not **one** failed login in the whole export — nothing was ever guessed | steps 3 and 16 |
| `grep -F 'User=jdoe'` returns **nothing at all** | step 8 |
| Searching for the bare name `jdoe` returns **248** records, because a backup service copies that user's files | steps 8 and 16 |
| **300,000 bytes** returns **7** connections, and six of them are ordinary internal file copies | steps 14 and 15 |
| **43** programs ran on the workstation the account used — but only **4** ran as that account | step 12 |
| The **43** and the **4** are the point: a busy machine is not a finding | step 12 |
| The hour `02:00` holds **31** records in the Security log and **nothing at all** in the resolver or the lease table | step 13 |
| The firewall's only verdict on the outbound transfer is **ALLOW**, which is a gap in your controls | steps 11 and 16 |
| **All 300** refused connections are three services retrying three dead ports. There is **no port scan** in this lab | step 15 |

## Finish and submit

1. Answer all 30 factual questions in the last step.
2. Press **Check my answers**. It compares your answers with the evidence and marks anything
   that disagrees. This is a learning aid, not your grade — a full 30/30 is the expected result
   once you have read the logs.
3. Write the two longer answers in your own words. Your teacher marks those.
4. Fill in your name and student ID.
5. Press **Export PDF report**.
6. Submit the exported file. Name it `beginner-offhours-access-001-<your-name>.pdf`.

Three of the thirty answers are **zero**, and they are three of the most valuable answers on the
page: nothing ever failed, nothing was ever looked up, and nothing was ever a port scan. Write
zero down as an answer. A zero with no explanation reads as a broken search.

## Stop and resume

Press `Ctrl+C` in the launcher window to stop the lab. Your step, your notes, your answers and
the list of commands you ran are saved in the browser, so you can close everything and come back
later.

To start over from scratch, clear the lab's saved progress in your browser: open the page, press
the **?** button and use **Reset progress**. To wipe it at the source, clear your browser's site
data for `127.0.0.1`.

## Troubleshooting

| What you see | What it means | What to do |
|---|---|---|
| Pill says **Offline** | the page cannot find a running lab | start the lab with `start-lab.desktop`; if it is already running, reload the page |
| **404** in the browser | you opened the wrong address | use the URL printed by the launcher, ending in `/` |
| The browser opened on the **host**, not in the VM | the browser that launched is the host's | close it, and open the URL manually in the browser inside Kali |
| **Address already in use** | another lab is already running on that port | stop the other one with `Ctrl+C`, or start this one on a different port: `./start-lab.sh 9000` |
| `python3: command not found` | Python 3 is not installed | `sudo apt update && sudo apt install -y python3` |
| **Permission denied** when you run the launcher | the launcher is not executable | `chmod +x start-lab.sh` and run it again |
| The terminal panel is blank | the shell has not connected yet | wait a few seconds; check the pill says **Real bash connected** |
| Double-clicking the launcher does nothing | the desktop environment may not trust it | right-click it and choose **Run in Terminal**, or run `./start-lab.sh` yourself |
| The rail says **UTC+08** but your log files say `+00:00` | your copy is from an older build of this lab | re-clone the repository and check `wc -l *.log \| tail -1` again |

## What is in this folder

| File | What it is |
|---|---|
| `README.md` | this file |
| `guided-walkthrough.html` | the walkthrough. Generated — do not edit it |
| `serve.py` | the lab server: serves the page and gives you a real shell |
| `start-lab.sh` | the launcher you run |
| `start-lab.desktop` | the launcher you double-click |
| `vendor_xterm.js`, `vendor_xterm.css`, `vendor_xterm_fit.js` | the terminal component, served locally so the lab works with no internet |
| `security_auth.log` | Windows Security events — where the whole incident is |
| `network_flows.log` | firewall connection records |
| `sysmon_process.log` | Sysmon file-write records |
| `dns_queries.log` | name lookups |
| `dhcp_leases.log` | which device owns which address |

**The five `.log` files are read-only evidence.** They are the record of what happened. Do not
edit them, and do not delete them. If a command does not work, read the step again — the answer
is in there, not in the file.