# TakeMeter
A fine-tuned model that determines the discourse between the online subreddit community of csMajors.

## Community choice and reasoning
I chose r/csMajors because there has been a rise of anxiety over the current job market amongst CS students and experienced professionals. Due to this, the community receives a lot of regular posts seeking advice to stand out in the job market, questions about what is expected of CS majors, expressions of anxiety and rants over their future, or expressions of their opinions regarding the future of programming and software engineering. This community is therefore a good fit for a classification task because of the repetitive patterns followed by the posts. By classifying the posts in this subreddit, I will be able to analyze the differences in the posts better and understand the regular patterns between posts. This will benefit the community as it helps them better understand the recent trends in people's attitudes towards CS in the subreddit.

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
1. 12 out of 17 wrong predictions were misclassified as advice_seeking. Looking at the confusion matrix, advice_seeking absorbed errors from every other label — 4 opinions, 4 rants, and 6 questions all got pulled into it. The model has essentially learned that advice_seeking is a "safe default" and is over-applying it, likely because it saw more advice-seeking-style language patterns (questions, first-person, uncertain tone) that also appear in your other labels.
   
2. Question and advice_seeking are heavily confused, exactly as I predicted
6 out of 10 question posts got misclassified as advice_seeking. This confirms the edge case I flagged in your planning.md. Both labels are phrased as questions, both use first person, and both have uncertain/curious tone, which causes a high structural similarity that is too subtle for the model to pick up on with limited data.

3. 'opinion' posts that give advice get misclassified. This is a direct consequence of the definition of my opinion label that explicitly includes "personal anecdotes of people who might be giving advice." When the word advice was mentioned in these personal anecdotes, the model was more likely to flag the text as advice_seeking, even if the text was not asking for advice, since the text used the word advice while giving advice to others.

**Overall accuracy:**


## Baseline model reflection
Reflect briefly: where did the baseline struggle? Are there specific labels it consistently confuses? Write down your hypothesis — you'll test it after fine-tuning.
The baseline model did a pretty good job of categorizing the posts without fine-tuning. The macro F1 score was 0.76 which is greater than 0.75. This means that the model was correctly identifying and capturing the right posts for each label 75% of the time for all labels averaged. The F-1 score for question was the lowest, at 0.64, because of all examples the model predicted as 'question', only 58% of them actually were question. 

High recall, low precision means the model over-predicts these labels. The precision was 1.0 for rant and question. This means that of all examples the model predicted as these labels, 100% of these actually were. High precision, low recall means the model is conservative — it only predicts this label when confident, but misses many real examples. Since the F1 score (harmonic mean of precision and recall) were all above 0.70, the model is learning all distinctions well.

## Hyperparameters changed for fine-tuning
1. num_train_epochs=3 → increased to 7. 
With only 200 examples, the model doesn't see enough data in just 3 passes to learn well. More epochs give it more chances to pick up patterns without needing more data.

2. per_device_train_batch_size=16 → decreased to 8. 
With ~160 training examples, a batch size of 16 means only 10 update steps per epoch, which is very few. Dropping to 8 doubles the update steps, giving the model more chances to adjust its weights each epoch.

3. warmup_steps=50 → decreased to 20. 
Warmup steps should be a small fraction of the total training steps. With a small dataset I have fewer total steps, so 50 warmup steps is too large a proportion — it means the model spends too long at a low learning rate before it starts learning properly.

4. weight_decay=0.01 → increase to 0.1. 
Small datasets are more prone to overfitting. Increasing weight decay adds stronger regularization to discourage the model from memorizing your 200 examples instead of learning generalizable patterns.


##README documents the data source, labeling process, and label distribution (count per label).

##README names the base model and the training platform (e.g., Colab, local, HuggingFace AutoTrain).

##At least one key training decision is described and justified — e.g., number of epochs, learning rate, or batch size, with reasoning or observation (not just "I used the default").

## AI Usage

1. I used AI to generate 10 sample hard edge cases that I tried to label. I was able to label them all without too much stress, showing that my definitions did not have overlap.
2. I used AI to go over my CSV file and find any text that had an inappropriate label. I read the posts that Claude gave back to me, and decided whether I had actually mislabeled something. Thanks to this, I was able to spot 4 texts that I mislabeled.
3. I al

## README describes the baseline approach — the prompt used and how results were collected.
