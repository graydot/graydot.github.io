### 🚀 What I’ve Learned Over the Past Few Days While Developing an AI SRE Agent

The past few days working on the AI SRE Agent have been a whirlwind of new discoveries, and I’ve picked up quite a few valuable lessons along the way. Here’s a quick rundown of what worked, what didn’t, and some cool things I learned.

#### 🤖 A Hybrid Approach: AI for Planning, Humans for Execution
At first, we had this idea that the AI could take the reins on everything—figuring out what needs to be done, choosing which steps to run, and handling execution. While it sounded great in theory, we sometimes ran into problems like the AI running forever (and hitting langgraph recursion limits!) and unpredictable outcomes (deciding it really really wanted to investigate all logs ever produced in the app). So, we changed things up. Now, the AI handles the planning, and deteministic code is used whenever possible. This approach gave us more predictable and, honestly, much more useful results. This is not news, it is best practice in most products that are currently built on LLMs. They tend to not halt and aggressively continue to investigate.

#### ✨ The Power of Being Specific with AI Instructions
One lesson I really took to heart is the importance of being super clear with the instructions you give to AI. Even a tiny bit of vagueness can throw things off track. A fun experiment was using other LLMs to help improve our prompts. They caught things like weird grammar or instructions that could be interpreted in more than one way. Having one AI improve the prompts for another? That’s some next-level stuff. I had this instruction which said 'get the `patch` from the commit' which was leading to unpredictable results. The LLM was returning random stuff in the field till I changed it to `diff`

#### 📑 YAML > JSON for Contextual Memory
This one took me by surprise. When providing context to the AI, we saw a huge improvement in the quality of responses when we switched from JSON to YAML. YAML’s cleaner, more human-friendly format seemed to help the AI make better sense of the context we were giving it. Who knew a small change in format could have such an impact? It also seems to help with performance as json seems to take longer to parse.

#### 🧩 Break Things Down into Smaller Steps
Instead of dumping a huge chunk of data on the AI all at once, breaking it down into smaller, manageable chunks worked much better. We’d start by summarizing API results, pass those summaries to another LLM for analysis, and then use that final summary for deeper insights. The same applied to commit analysis—summarize individual commits first, summarize those summaries, and then use it all for analysis. This step-by-step method kept things clear and effective.

#### 🐶 Datadog Is a Beast (But It’s a Bit Tricky)
On the non-LLM side, I spent some time exploring Datadog, and wow, that tool is powerful. It has a ton of features, which is great, but navigating the API wasn’t as smooth as I hoped. The metric filter language took a little getting used to, but once I figured it out, it wasn’t so bad. The one downside was that Datadog doesn’t give you an easy way to generate links to the Metric Explorer. But hey, the snapshot API was pretty awesome, and the pre-built graphs saved a lot of time.

#### 🌿 Celery Advanced Stuff Is Pretty Cool
I also dove into some of Celery’s advanced features, like chords, groups, and callbacks. I ended up using the `group(tasks) | callback.s()` style, which turned out to be way cooler than I expected. Celery is great for handling complex workflows and makes managing asynchronous tasks a lot more fun.

#### 🐞 Debugging an Annoying Bug
One of the more frustrating challenges I ran into was getting the LLM to return inline content for important artifacts. It would return URLs fine, but the inline content was nowhere to be found. After digging through the code, I realized the problem was with the schema I was using. I had defined a `dict` field for inline content in a `Resource` object, but OpenAI didn’t like that. A quick fix—switching to an `InlineContent` field—did the trick. It took me a day to figure it out as I had to take apart each part of the llm call, but honestly, I was pretty proud of myself for not giving up. The problem turned out to be in a place which seemed quite standard and battle tested. Persistence wins again.

#### 👏 The Team Made It Happen
At the end of the day, this wasn’t just me—my team worked their butts off too. The last two weeks were intense, but seeing everything come together in the final product made it all worth it.

🎥 Check out the demo by our Head of Developer Relations at the [recent AI panel](https://lu.ma/9wi116nk) [here](insert-url-here). This is a monthly event so drop by meet the crew, I will be there too. 
