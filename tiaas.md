---
title: Approving TIaaS Requests
---

# Approving TIaaS Requests

Check for rows with no value in the **Processed** column. For each of these rows:

- Look at the trainings that will be run, is it RNA-seq, is it something else. This can give you an idea of the maximum memory they will request at one time, for one tool run, but it does not tell the whole story.
  - You don't know what time limitations they have, maybe they expect a step to run in 1 minute, but we don't know that.
  - You don't know if they'll run a dataset collection. That tool which took 2 cores/8GB now takes 10 times that, and they have 30 students running it at once.
  - Basically: there is no hope to guess the number correctly without more information that isn't so easy to collect.
- We generally assign c.c32m240 machines
  - you can maximally assign up to 8
  - That flavour is more or less only used for training, so no changes to main queue needed.
- Select the start/end date columns and ensure they are formatted to `YYYY-mm-dd`
- You're ready to run the script!

## Finding to-be-approved trainings

- [Go here](https://usegalaxy.eu/tiaas/admin/training/training/?processed__exact=UN)
  These are all unprocessed.
- Click on one training
- This brings you to the edit-tiaas-request page
- Check their request
- At the bottom, change **Processed** to Approved
- Save

you've approved it in our system, but now you need to allocate resources!

## Running the script

Ensure that you have https://github.com/usegalaxy-eu/vgcn-infrastructure cloned.

Go into `vgcn-infrastructure`, and run `./add-training.sh`. It will print out the arguments you need to supply (I do this every time because I never remember.)

Copy and paste values from the spreadsheet into the correct places in the command line, and run it. The script will do a few things:

- adds an entry to [`resources.yaml`](https://github.com/usegalaxy-eu/vgcn-infrastructure/blob/master/resources.yaml) with the information for the training
- commits and pushes
- print out an email you should send the person.

## Sending the email

Copy and paste it into your email, and send it. Mark the row as 'processed' in the spreadsheet.

## What happens in the background

- The next time the galaxy playbook runs, it deploys the tiaas training. Then the URL becomes active.
- Whenever `todays_date >= start date`, vgcn-infrastructure will automatically **try** and launch the VM.

# FAQ

Question | Answer
--- | ---
What if there are multiple trainings? | Has not happened yet. If it has, it is with long running trainings and we usually give them fewer / smaller machines.
