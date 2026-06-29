# TakeMeter
A fine-tuned model that determines the discourse between the online subreddit community of csMajors.

## Community choice and reasoning
I chose r/csMajors because there has been a rise of anxiety over the current job market amongst CS students and experienced professionals. Due to this, the community receives a lot of regular posts seeking advice to stand out in the job market, questions about what is expected of CS majors, expressions of anxiety and rants over their future, or expressions of their opinions regarding the future of programming and software engineering. This community is therefore a good fit for a classification task because of the repetitive patterns followed by the posts. By classifying the posts in this subreddit, I will be able to analyze the differences in the posts better and understand the regular patterns between posts. This will benefit the community as it helps them better understand the recent trends in people's attitudes towards CS in the subreddit.

<div>
    <a href="https://www.loom.com/share/69b8124afd7d4e43b461197d63d79b50">
      <p>Tachymeter Project 3: Baseline and Fine-Tuned Results - Watch Video</p>
    </a>
    <a href="https://www.loom.com/share/69b8124afd7d4e43b461197d63d79b50">
      <img style="max-width:300px;" src="https://cdn.loom.com/sessions/thumbnails/69b8124afd7d4e43b461197d63d79b50-ab0bed13acea392f-full-play.gif#t=0.1">
    </a>
  </div>

## Data collection 
**Source**: Reddit's posts and comments in the r/csMajors subcommunity
**Label distribution:** 
1. opinion:77
2. advice_seeking:84
3. rant:75
4. question:68

## Fine-tuning approach: base model, training setup, and at least one hyperparameter decision
**Base model:** llama-3.3-70b-versatile
**Training setup:** The training platform was Colab. First DistilBERT was loaded with a classification head. Then the hyperparameter training arguments were decided upon according to the size of the dataset. Then, we run the inference on the test set, after which the per-class metrics are calculated from which the confusion matrix is generated.
**Epoch hyperparameter:** Epoch is chosen to be 3 since my dataset is small and has 304 rows. Having an epoch greater than 3 risks the model overfitting. To prevent this, 3 is a standard epoch value that I chose not to change.

## Baseline description: prompt used and how results were collected
**Prompt used:**
"""
You are classifying posts and comments from the subreddit r/csMajors.
Assign each post to exactly one of the following categories.

rant: A label that represents people expressing their concerns and anger over recent CS events.
Example: "I'm losing all hope in the job search - need motivation I've just come up on a year of job hunting since I graduated last year for Data Science and after hundreds of job applications, I've gotten a grand total of 2 interviews, both of which went nowhere. I thought I did everything right. I went to a top school, had a good gpa and some solid club experience (nothing crazy but still), an internship (no research though), and am a US citizen who thankfully didn't have to deal with the international struggle. I accepted an analyst position after graduation, which I thought would maybe help me, but all I see around me are friends and people I graduated with earning literally 1.5x-2x as much as I am making post grad, in reputable companies with a ton of growth potential and responsibility. Meanwhile, i sit around begging for work half the time, and am gaining such little valuable experience with barely any opportunity for growth. I've tried every option to get my resume reviewed, and I'm so confident that it's tight, so I have no idea if it's just something wrong with me. I'm at my wits end, and I just keep thinking ""what's the point"" every time I submit another job application. This might be a far cry, but if anyone has motivation or inspiration from their own lives or people they know, especially if they had a similar struggle to me, I'd really appreciate it :) I really need a reason to not give up."

opinion: A label that represents people expressing their beliefs, thoughts, and opinions regarding CS.
Example: "Supply & demand. The bar raised mainly because companies are shedding workers en masse now that interest rates are higher, and to a lesser extent due to AI. They'll need new blood eventually, but supply & demand will rule your entire career. That's why I'd focus on learning in-demand skills that others aren't, and that aren't so easily outsourced."

question: This label represents questions people have about the CS major in general.
Example: "Would I'd be considered underemployed if I joined the army as a cyber operations specialist? The title basically. I will graduate with a software engineering degree and I'm done with leetcode, I'm just not fast, I can understand the questions and find solutions, but it takes me too long to write them, also sometimes I straight up just don't understand how the solution comes to be sorted in the way presented. Also my math is not mathing, for every 200 apps I might've gotten 2 interviews. I already took the asvab and got an 88, for those saying join as an officer, you don't pick your job, and the application process is very selective. I also discarded the space force and the air force for this very motive edit: (you don't pick your job)."

advice_seeking: A label that represents people asking for advice on how to improve themselves in CS.
Example: "Should I make a reasearch based fyp or product based fyp?? Graduating next year 2027"

Respond with ONLY the label name.
Do not explain your reasoning.

Valid labels:
<rant>
<opinion>
<question>
<advice_seeking>
"""

**Results collection**: Results were collected by running the baseline on the test sets and then printing the per-class metrics classification report based on the test cases that were categorized correctly and incorrectly.

## Evaluation Report
**Patterns:** 
1. 5 out of 11 wrong predictions were misclassified as advice_seeking. The model has essentially learned that advice_seeking is a "safe default" and is over-applying it, likely because it saw more advice-seeking-style language patterns (questions, first-person, uncertain tone) that also appear in my other labels.
   
2. Question and advice_seeking are heavily confused, exactly as I predicted
3 question posts got misclassified as advice_seeking. This confirms the edge case I flagged in my planning.md. Both labels are phrased as questions, both use first person, and both have uncertain/curious tone, which causes a high structural similarity that is too subtle for the model to pick up on with limited data.

3. 'opinion' posts that give advice get misclassified. This is a direct consequence of the definition of my opinion label that explicitly includes "personal anecdotes of people who might be giving advice." When the word advice was mentioned in these personal anecdotes, the model was more likely to flag the text as advice_seeking, even if the text was not asking for advice, since the text used the word advice while giving advice to others.

**Overall accuracy:**
"baseline_accuracy": 0.7609,
"finetuned_accuracy": 0.7609,
"improvement": 0.0,
"test_set_size": 46

## Baseline Per-Class Metrics

| Label          | Precision | Recall | F1-Score | Support |
|----------------|-----------|--------|----------|---------|
| opinion        | 0.91      | 0.83   | 0.87     | 12      |
| advice_seeking | 0.88      | 0.58   | 0.70     | 12      |
| rant           | 0.73      | 0.92   | 0.81     | 12      |
| question       | 0.58      | 0.70   | 0.64     | 10      |
| **accuracy**   |           |        | **0.76** | 46      |
| macro avg      | 0.78      | 0.76   | 0.76     | 46      |
| weighted avg   | 0.78      | 0.76   | 0.76     | 46      |

## Fine-Tuned Model Per-Class Metrics

| Label          | Precision | Recall | F1-Score | Support |
|----------------|-----------|--------|----------|---------|
| opinion        | 0.88      | 0.58   | 0.70     | 12      |
| advice_seeking | 0.69      | 0.92   | 0.79     | 12      |
| rant           | 0.77      | 0.83   | 0.80     | 12      |
| question       | 0.78      | 0.70   | 0.74     | 10      |
| **accuracy**   |           |        | **0.76** | 46      |
| macro avg      | 0.78      | 0.76   | 0.76     | 46      |
| weighted avg   | 0.78      | 0.76   | 0.76     | 46      |


## Confusion Matrix

| True \ Predicted | opinion | advice_seeking | rant | question |
|-----------------|---------|----------------|------|----------|
| **opinion**         | 7       | 1              | 3    | 1        |
| **advice_seeking**  | 0       | 11             | 0    | 1        |
| **rant**            | 1       | 1              | 10   | 0        |
| **question**        | 0       | 3              | 0    | 7        |

## Examples that were predicted wrong by fine-tuned model

1.
ext:      I almost feel guilty.
I'm an intern at an embedded systems company doing full stack work for them. I do a ton of great work for them and, despite being able to use AI as much as I like, I have enough ...
True:      rant
Predicted: advice_seeking  (confidence: 0.32)

The above example was predicted as advice_seeking because at the end of the rant, OP asks for advice. According to my definition, if majority of the post is a rant, it should be classified as a rant. However, with the small dataset, the model did not get enough examples to see that word count is what matters in the classification, due to which, it classified this post as advice_seeking.

2.
Text:      What would be the one sentence advice you can give to freshman/sophomore's?
True:      advice_seeking
Predicted: question  (confidence: 0.48)

The above example was predicted as question. This post is really short, due to which, the model did not get to analyze enough words to be able to tell that the purpose of this post was to gain advice that would help OP as a freshman/sophomore. The model was used to seeing texts in the dataset that had a lot of personal context before the advice question was asked. However, since this does not have that context, the model thought it was a question.

3.
Text:      Honest question for people in industry.
I’m a CS student at UNC Charlotte (UNCC), which is a solid school but not a top / prestige CS program. By the time I graduate, I’ll have around 5 years of profe...
True:      question
Predicted: advice_seeking  (confidence: 0.32)

The above example is predicted as advice_seeking because of the amount of personal context given in the question. OP is however not seeking advice, since the question asked at the end does not help OP improve themselves in any way. The question is a general question about how much prestige a school holds. Since this is not asking for advice, it is not supposed to be labeled as advice_seeking. However, because most of the advice_seeking examples contained a lot of personal context, the model thought this was seeking for advice too. The model did not get enough data to train itself properly on what kind of context counts as context for advice.

To fix these wrong predictions, a bigger dataset is required to give the model more examples about edge cases and how to go about labeling them.

## Sample classifications
| Text (truncated) | True Label | Predicted Label | Confidence | Reason |
|------------------|------------|-----------------|------------|--------|
| "Where is the evidence that the hiring bar is significantly higher now? I'm not a new grad, but I've been doing interview prep for mid-level roles and I do competitive programming as a hobby..." | question | advice_seeking | 0.31 | |
| "What would be the one sentence advice you can give to freshman/sophomore's?" | advice_seeking | question | 0.48 | |
| "I started prepping for internships backwards and it's lowkey been working. sounds dumb but hear me out. if my goal is an Amazon internship, i don't start with 'grind leetcode.' i start with the deadline..." | opinion | rant | 0.46 | |
| "Weirdly I found the opposite actually. I had internships at popular companies but no real 'big tech' companies. I never really got interviews for their internships. But I did end up at one for new gra..." | opinion | opinion | 0.30 | This post shows the OP giving their opinion regarding how they got into big tech without interning there. Since this tells people about their methods and what they think is successful, it makes sense for this to be categorized as opinion. | 

## Reflection: Intended vs. Captured Behavior
When designing the label definitions, the goal was for the model to distinguish posts based on communicative intent — what the poster was trying to accomplish by writing. In practice, the model learned something shallower: surface language patterns. The clearest example is the opinion vs rant boundary, where the model frequently misclassified casual or negatively-worded opinion posts as rant, suggesting it latched onto informal language as a proxy for anger rather than actual emotional intensity. Similarly, the question vs advice_seeking boundary was drawn at whether a post sought personal improvement or general curiosity — meaningful to a human reader, but nearly invisible at the surface level. The model appears to have overfit to the presence of personal background information as a signal for advice_seeking, pulling genuinely curious questions across the boundary whenever they included personal context.

The advice_seeking label being the most overrepresented in wrong predictions suggests the model learned it as a safe default rather than a precise category, likely because it shares linguistic features with every other label — emotional like rants, opinionated like opinions, and question-structured like questions. What the model captured well was rant and advice_seeking in their most prototypical forms: long frustrated posts and direct requests for tips. What it consistently missed was nuance in tone and intent, especially in short posts where the defining signal of a label was weak or absent. The core gap is that the definitions were written at the level of communicative intent, but the model could only ever learn at the level of lexical and syntactic patterns — closing that gap would require more training data or definitions that translate intent into more surface-detectable features.

## Baseline model reflection (Reflect briefly: where did the baseline struggle? Are there specific labels it consistently confuses? Write down your hypothesis — you'll test it after fine-tuning.)

The baseline model did a pretty good job of categorizing the posts without fine-tuning. The macro F1 score was 0.76 which is greater than 0.75. This means that the model was correctly identifying and capturing the right posts for each label 75% of the time for all labels averaged. The F-1 score for question was the lowest, at 0.64, because of all examples the model predicted as 'question', only 58% of them actually were question. High recall, low precision means the model over-predicts these labels. 

## Spec reflection
In the spec, I initially intended to have 200 examples of posts. However, after realizing that this would mean each label would only get 50 data values, and only around 8-9 of each of these labels would be used for testing, I decided to add 100 more data rows to better understand where the model was going wrong. This is how my implementation diverged from the spec. However, the spec also helped me enforce each label's definition clearly. The hard edge cases reminded me, everytime I got confused, that I needed to account for which label seems to take up majority of the post. This simple reminded given by the spec helped me be consistent with most of my labels.

## AI Usage

1. I used AI to generate 10 sample hard edge cases that I tried to label. I was able to label them all without too much stress, showing that my definitions did not have overlap.
2. I used AI to go over my CSV file and find any text that had an inappropriate label. I read the posts that Claude gave back to me, and decided whether I had actually mislabeled something. Thanks to this, I was able to spot 4 texts that I mislabeled.
3. I also used AI to find patterns between the mislabeled posts. This helped me gain a better understanding of the deeper issues that were lying in the model.

## README describes the baseline approach — the prompt used and how results were collected.
