---
date: 2024-03-15
title: Bitten by timezones and Docker
snippet: |
  Today I learned you should explicitly set the TZ environment variable in Docker to prevent surprises.

  I found out while building a Telegram chatbot to remind me to take out the trash.
url: /2024/bitten-by-docker
---

I am building a Telegram chatbot to remind me to take out the trash.

I'm overengineering it, so of course it has Docker and a (sqlite) database.

One component of the chatbot does an hourly check. If tomorrow the trash will be collected, and the current time is later than the time I want to receive the notification (and if I haven't been notified today), I should get a message.

However, I was not getting the messages. The code seemed solid

{{< highlight sql >}}
SELECT 
  user 
FROM
  users
WHERE
(
  TIME('now', 'localtime') > TIME(time_of_day) AND
  DATETIME('now', 'localtime', '-1 day') >= 
  DATETIME(last_executed_run, TIME(time_of_day))
)
OR
  last_executed_run IS NULL
{{< /highlight >}}

Worse of all, when I was running on the Docker host, I would get notified at the right time.

It turns out Docker doesn't respect the host timezone, but you need to be explicit about it, adding a `TZ` environment variable (in my case `TZ=Europe/Amsterdam`).
