---
title: "Debugging Notes – 3/26"
date: 2025-03-26
summary: "A debugging session involving Langfuse and Celery, highlighting the importance of understanding runtime environments."
tags: [debugging, langfuse, celery, python, observability, sre]
reading_time: 2 minutes
---
# Debugging Notes – 3/26

Today's debugging session was a reminder of how important it is to understand your runtime environment.

This morning, I started integrating Langfuse into the SRE pipeline. It worked perfectly in a small, isolated prototype script but failed in the actual worker. I tried everything—even dropping in a debugger—but nothing worked. Traces showed up in the Langfuse dashboard when I manually entered code from the prototype, but nothing appeared when using the script.

Then, during a short break, I realized I was using Celery. That meant background jobs, which the prototype didn’t use. A quick search led me to this GitHub issue. I moved the Langfuse initialization from the async caller to the agents' base class, and the traces finally started showing up.

Zooming out helped me reframe the problem—and stepping away for a few minutes didn’t hurt either.
