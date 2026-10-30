# Cloud Architecture Design Case Study: Progress Tracker

**Course:** BCSE355L, Cloud Architecture Design (A2 slot)
**Student:** Pawan Kumar (24BCT0153)
**Repo:** https://github.com/HwkShadow/cloud_aws
**Time budget:** about 4 hours
**Last updated:** 2026-10-30

## Goal

Prototype web app on the AWS Free Tier:

- Frontend: static HTML/CSS/JS in a public S3 bucket
- Backend: two EC2 instances (Server1, Server2) running dynamic and database logic
- An Application Load Balancer (ALB) in front of Server1 and Server2

## Decisions

| Item | Choice |
|---|---|
| Region | Asia Pacific (Mumbai), ap-south-1 |
| Account plan | Credit-based free plan |
| Backend stack | Python Flask |
| Database | MySQL on Server1 (Server2 connects over private IP) |
| Load balancer | Application Load Balancer, HTTP:80 |
| Approach | Hybrid: code generated, AWS console steps done by hand with screenshots |
| S3 bucket name | `pawan-24bct0153-cloud-frontend` (change if taken) |

## Architecture

```
Browser
  |
  +--> S3 static website (public)       [HTML/CSS/JS]
  |
  +--> ALB (HTTP:80, 2 AZs)
         |-- Server1 (EC2, Flask, MySQL)
         '-- Server2 (EC2, Flask) --> MySQL on Server1
```

## Parts

Legend: [x] done, [~] in progress, [ ] not started

### Part 1: Setup (15 min)
- [x] Check free tier status (credit-based plan)
- [x] Set region to Mumbai (ap-south-1)
- [ ] Architecture diagram (draw.io or generated)

### Part 2: Frontend on S3 (30 min)
- [x] Frontend code written (`frontend/index.html`)
- [~] Create S3 bucket, turn off Block Public Access
- [ ] Upload `index.html`
- [ ] Enable static website hosting
- [ ] Add public-read bucket policy
- [ ] Open the website endpoint and confirm the page loads
- [ ] Screenshots: public access off, bucket policy, hosting endpoint, page in browser

### Part 3: Security groups (20 min)
- [ ] ALB-SG: HTTP 80 from anywhere
- [ ] EC2-SG: HTTP 80 from ALB-SG, SSH 22 from my IP
- [ ] DB-SG (or rule): 3306 from EC2-SG only
- [ ] Screenshot of the three groups and their rules

### Part 4: Backend code and user-data (20 min)
- [ ] Flask app: `/health`, `/api/data`, `POST /api/notes`, CORS
- [ ] Server1 user-data (installs MySQL, app, starts service)
- [ ] Server2 user-data (installs app, points to Server1 DB)

### Part 5: Launch Server1 and Server2 (40 min)
- [ ] Launch Server1 (t3.micro, AZ a)
- [ ] Launch Server2 (t3.micro, AZ b)
- [ ] Verify `/health` on each instance directly
- [ ] Screenshot: both instances running

### Part 6: Target group and ALB (30 min)
- [ ] Target group (HTTP:80, health check `/health`), register both servers
- [ ] Create ALB across 2 AZs, listener 80 to the target group
- [ ] Screenshot: target group with 2 healthy targets

### Part 7: Connect and test (25 min)
- [ ] Put ALB DNS into `API_BASE` in `index.html`, re-upload to S3
- [ ] Refresh and confirm responses alternate between Server1 and Server2
- [ ] Failover test: stop one instance, confirm the site still works
- [ ] Screenshots: alternating log, failover

### Part 8: Write-up and cleanup (35 min)
- [ ] Case-study doc with diagram and screenshots
- [ ] Push everything to the GitHub repo
- [ ] **Delete the ALB, target group, and terminate EC2 instances** (credits)
- [ ] Empty and delete the S3 bucket if no longer needed

## Suggested repo layout

```
cloud_aws/
  PROGRESS.md
  frontend/index.html
  backend/app.py
  userdata/server1.sh
  userdata/server2.sh
  docs/screenshots/
```

## Notes

- Never commit AWS keys or DB passwords. Use placeholders in the repo.
- The ALB is the main cost item, so delete it right after taking screenshots.
- The S3 website endpoint is HTTP only, so the ALB stays on HTTP to avoid mixed-content blocking.

## Issues log

| Date | Issue | Fix |
|---|---|---|
| | | |
