# TakeMeter
A fine-tuned model that determines the discourse between the online subreddit community of csMajors.

## Baseline model reflection
Reflect briefly: where did the baseline struggle? Are there specific labels it consistently confuses? Write down your hypothesis — you'll test it after fine-tuning.
The baseline model did a pretty good job of categorizing the posts without fine-tuning. The F1 score was greater than 0.75 for all labels (opinion, advice_seeking,  rant) except for question, that had an F1 score of 0.73. The recall was 1.0 for opinion and advice_seeking, which means that of all examples that truly were of these labels, the model caught 100% of these. High recall, low precision means the model over-predicts these labels. The precision was 1.0 for rant and question. This means that of all examples the model predicted as these labels, 100% of these actually were. High precision, low recall means the model is conservative — it only predicts this label when confident, but misses many real examples. Since the F1 score (harmonic mean of precision and recall) were all above 0.70, the model is learning all distinctions well.

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

## README describes the baseline approach — the prompt used and how results were collected.
