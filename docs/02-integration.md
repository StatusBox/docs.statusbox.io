---
layout: default
title: Integration
nav_order: 2
---

# Integration
StatusBox provides simple REST API for submiting created jobs, events and status updates. For each HTTP request you will need to include special secret ApiKey for authorization.

## Creating secret key
1. <a href="https://dash.statusbox.io/" target="_blank">Login or create free StatusBox account.</a>
2. In your acoount, **go to settings page**.
3. Create new secret by clicking on **generate new secret**.
4. Include your secret token into every HTTP request to StatusBox as an `Authorization` header with `ApiKey` schema.

## Creating new job
All you have to do is to send a POST HTTP request to StatusBox API after a job creation. You'll get an integer dynamic id for created job in the response from API so you can log events attached to that job or update its status. Here is an example with `curl`:

*Request*
```bash
curl --request POST 'https://g.statusbox.io/api/jobs/' \
--header 'Content-Type: application/json' \
--header 'Authorization: ApiKey YourGeneratedApiSecret' \
--data-raw '{
  "name": "Video transcoding job", 
  "timeoutSeconds": 300, 
  "version": "1.0.0"
}'
```

*Response*
```json
{"status":"ok","id":1}
```

| Parameter name   | Required | Type   | Description                                                             |
|------------------|----------|--------|-------------------------------------------------------------------------|
| `name`           | Yes      | String | Job name, max 64 characters                                             |
| `timeoutSeconds` | Yes      | Number | Expected job execution time after which job will be marked as timed out |
| `version`        | No       | String | Job version or any other custom marker                                  |

## Log an event
Now as we have job id, we can send some log messages that will be attached to it. Here is a simple example: 

*Request*
```bash
curl --location --request POST 'https://g.statusbox.io/jobs/:jobId' \
--header 'Content-Type: application/json' \
--header 'Authorization: ApiKey YourGeneratedApiSecret' \
--data-raw '{ "message": "Job has been created" }'
```

*Response*

Empty response with `200 OK` status code.

## Change job status

### Complete job
Signals that job has completed successfully.

*Request*
```bash
curl --location --request POST 'https://g.statusbox.io/jobs/:jobId/complete' \
--header 'Content-Type: application/json' \
--header 'Authorization: ApiKey YourGeneratedApiSecret'
```

*Response*

Empty response with `200 OK` status code.

### Cancel job
Signals that job has been cancelled by scheduler or other service.

*Request*
```bash
curl --location --request POST 'https://g.statusbox.io/jobs/:jobId/cancel' \
--header 'Content-Type: application/json' \
--header 'Authorization: ApiKey lwo9vxEnNwysf7s8AWHMGviWXvxbPK3Q0sEtlqWwRYawfjd3r7NnD7iyaz1CVNrh'
```

*Response*

Empty response with `200 OK` status code.

### Fail job
Signals that job has failed.

*Request*
```bash
curl --location --request POST 'https://g.statusbox.io/jobs/:jobId/fail' \
--header 'Content-Type: application/json' \
--header 'Authorization: ApiKey lwo9vxEnNwysf7s8AWHMGviWXvxbPK3Q0sEtlqWwRYawfjd3r7NnD7iyaz1CVNrh'
```

*Response*

Empty response with `200 OK` status code.
