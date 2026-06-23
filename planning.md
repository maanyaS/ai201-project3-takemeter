##Community: r/csMajors What community did you choose and why? Why is this community a good fit for a classification task — what makes the discourse varied enough to be interesting?
I chose csMajors because there has been a rise of anxiety over the current job market amongst CS students and experienced professionals. Due to this, the community receives a lot of regular posts seeking advice to stand out in the job market, expressing anxiety over their future, or expressing their hot takes regarding the future of programming and software engineering. This community is therefore a good fit for a classification task because of the repetitive patterns followed by the posts. By classifying the posts in this subreddit, I will be able to analyze the differences in the posts better and understand the regular patterns between posts. This will benefit the community as it helps them better understand the recent trends in people's attitudes towards CS in the subreddit.

##Labels: advice_seeking, hot_take, reaction What are your 2–4 labels? Define each in a complete sentence. Include 2 example posts per label.
**advice_seeking**: A label that represents posts that are asking for advice regarding their CS careers.
Example 1: 
"Should I make a reasearch based fyp or product based fyp?? Graduating next year 2027"

Example 2:
"What is expected of an intern?
For context I’m doing a backend software engineering internship at a fortune 500 company.

And lately it feels like the expectation is insane for someone still learning. I understand that internships are meant to be challenging, and I do want to contribute meaningfully. But at times it feels less like “learn while contributing” and more like I’m expected to operate close to a full-time engineer with much more experience. It’s kinda hell when you’re the only intern/junior level dev in the team too.

Additional context: i have merged reviewed request to main for a service that’s actively in production.

I’m working on backend tasks in prod, and have been questioned on my pace. Tbh I don’t mind being pushed, but I’m finding it hard to tell whether this is normal industry expectation or whether the bar is unusually high because AI exists.

TLDR:
For people who have been interns, mentored interns, or worked with backend interns:
What is a reasonable expectation for a software engineering intern?"

**hot_take**: A label that represents people expressing their beliefs regarding the future of CS or the present competition in the CS community.
Example 1: 
"Supply & demand. The bar raised mainly because companies are shedding workers en masse now that interest rates are higher, and to a lesser extent due to AI. They'll need new blood eventually, but supply & demand will rule your entire career. That's why I'd focus on learning in-demand skills that others aren't, and that aren't so easily outsourced."

Example 2: 
"Right. Honestly, I dare to say that people were more competent back then, because we did not have AI as juniors.
There's something about recent graduates nowadays, where they overestimate how competent they are.
But the job market sucks, that's true."

**rant**: A label that represents people expressing their concerns over recent trends that are taking place in their careers.
Example 1:
"I'm losing all hope in the job search - need motivation
I've just come up on a year of job hunting since I graduated last year for Data Science and after hundreds of job applications, I've gotten a grand total of 2 interviews, both of which went nowhere.

I thought I did everything right. I went to a top school, had a good gpa and some solid club experience (nothing crazy but still), an internship (no research though), and am a US citizen who thankfully didn't have to deal with the international struggle. I accepted an analyst position after graduation, which I thought would maybe help me, but all I see around me are friends and people I graduated with earning literally 1.5x-2x as much as I am making post grad, in reputable companies with a ton of growth potential and responsibility. Meanwhile, i sit around begging for work half the time, and am gaining such little valuable experience with barely any opportunity for growth.

I've tried every option to get my resume reviewed, and I'm so confident that it's tight, so I have no idea if it's just something wrong with me.

I'm at my wits end, and I just keep thinking ""what's the point"" every time I submit another job application.

This might be a far cry, but if anyone has motivation or inspiration from their own lives or people they know, especially if they had a similar struggle to me, I'd really appreciate it :) I really need a reason to not give up."

Example 2:

##Hard edge cases: What type of post will be genuinely ambiguous between two labels? How will you handle it when you encounter it during annotation?
A post that combines a mix of two of the labels or even three will be ambiguous to differentiate. Some posts express hot takes, reactions to the hot takes or recent event, and then advice on how to not be a victim of the event or hot take, all in a single post. I will handle this by observing what the main focus of the post is. If the post focuses more on receiving advice, then I will categorize it as advice_seeking. If the post focuses more on the reaction to an event, I will label it as reaction. If a post focuses more on hot takes that have no evidence to back them up, or if the evidence is cherry picked, I will label it hot_take. My labels will mainly be based upon what aspect of the post is most prominent.

##Data collection plan: Where will you collect examples? How many per label? What will you do if a label is underrepresented after 200 examples?

##Evaluation metrics: Which metrics will you use to evaluate your model and why are those the right ones for this specific task? (Accuracy alone is not enough — explain what else you need and why.)

##Definition of success: What performance would make this classifier genuinely useful? What would you accept as "good enough" for deployment in a real community tool?

Add an ##AI Tool Plan section to your planning.md. This project's workflow is different from implementation projects — there's no code to generate, so your plan should cover the three places where AI tools actually help here:

Label stress-testing: Give the AI your label definitions and edge case description, and ask it to generate 5–10 posts that sit at the boundary between two labels. If it produces posts you can't classify cleanly, your definitions need tightening — do that now, before you annotate 200 examples.
Annotation assistance: Decide whether you'll use an LLM to pre-label a batch of examples before reviewing them yourself. If yes, note which tool you'll use and how you'll track which examples were pre-labeled (for disclosure in your AI usage section).
Failure analysis: Plan to give your list of wrong predictions to an AI tool and ask it to identify patterns before you write up your evaluation. Note what you'll look for and how you'll verify the patterns yourself.
