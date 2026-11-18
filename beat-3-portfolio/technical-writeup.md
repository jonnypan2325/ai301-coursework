# 

## Overview
The scout is able to help me scour GitHub for open issues in Open Source Repos that are relevant to my skillset and preference.
I was able to accomplish that by having Claude analyze my GitHub hisotry and resume to produce a tailored fit profile and search strategy to look for valid issues.


## What happened
At the very start, the Claude Skills that I have created for the issues selection was too restrictive, and only gave me very limited issues. When I made the rules more lenient, I ended up getting too many issues.
One change that I made was instead of rating the issue on active-ness (most recent comment on issue), I rated it on the repo itself, as more often than not, a issue with a recent comment actually means someone else claimed that issue before me.
I ensured that my AI usage complied with all open source repo's AI disclosure and agreement.
In the end, I was able to find a very fitting frontend issue on AntAlmanac, which is a course planner for students.
## Limitations and what's next
The limitations of my skills include bot pushes count as activity in the repo, which is inaccurate, since a repo's health should only be based on maintainers and contributors, not bots. Also, I need to ensure the skills change as my preferences and experiences change, and that it's not static. Next steps would be adding more skills and automating the skills via hooks + workflow so I'm able to be notified about new issues without me having to run the skills manually.
