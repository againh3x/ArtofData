# Introduction

MIMIC-IV is a large open-source database on Physionet that offers ED and ICU data from the Beth Israel Deaconess Medical Center. The data comes in multiple large files, with each file corresponding to some sort of patient or item identification, usually in the form of a subject_id or hospital admission id. This allows for data to be linked across files and projects, for example linking a patient’s diagnosis to their length of stay in the ED. 

My goal in this project was to use the vast amount of ED and ECG data offered, along with my previous experience working with ECGs to create a model that can predict how long a patient presenting with chest pain will stay in the ED for after the time of their most recent ECG taken. The model should input either the raw ECG, calculated ECG features, and tabular features that I can find from other modules that include acuity, mode of transport to the hospital, vitals, etc.

---

# Preprocessing

I first wanted to get my cohort of patients, so I filtered all subject id’s that were linked to presenting with chest pain or cardiac syncope. After this, I linked the patient id to the MIMIC-IV-ECG dataset, to search for whether they had an ECG taken after their admission time and before their discharge time. I then created the ECG_Length_of_Stay variable, which calculates the length of stay in the ED after the ECG was taken. 

After this, I linked the admission of the patient to their vitals in the vitals file, and updated their vitals with the most recent vitals before the corresponding ECG and ECG_LoS. I also added more tabular variables including mode of transport and acuity. I one-hot-encoded these variables, which essentially means turning them (strings) into boolean columns for each answer given. 

MIMIC-IV-ECG also provides machine measurements for each ECG, which include numerical measurements, such as calculated wave intervals or amplitudes. However, they also include machine-generated reports, which could be “ST-elevation” that often indicates the person having a heart attack. However, there were a ton of reports, and they were listed under 17 columns. Thus, after I one-hot-encoded these reports, I got over 1500 columns (this is because all unique possible strings become a different column). 

To reduce this, I first removed excessive “ornaments,” such as underlines, dashes, or other punctuation that was causing columns with the same meaning/words to be recognized as different. I then re-combined equivalent columns and repeated this process. However, a lot of different reports correspond to very similar clinical implications, so I worked with online sources, my previous knowledge, and asked for help from my aunt (a cardiologist) to reduce it to around 100 columns after a ton of work.

Next, I inputted all the tabular data and the edited machine generated reports into a few machine learning algorithms, which gave me poor accuracies. An XGBoost model gave me around a 0.77 AUC (not good, not bad) in predicting whether or not a patient would stay for over 8 hours. However, the vast majority of patients in my data stayed for more than 8 hours, which definitely made it harder to predict. I’ll work on trying regression models or different hour ‘markers’ in the future.

---

# My Current Plan

Now, my goal is to use a residual neural network to use far more computation and predict the LoS better. Firstly, I am “pretraining” a ResNet to simply be more familiar with ECGs and understand ECG patterns (without inputting any LoS labels). Then, my goal will be to fine-tune that pretrained model based on LoS and have it try to predict that. 

In order to pretrain the model, I gathered the full dataset of 800,000 ECGs, with their corresponding reports and my previous pipeline for editing and making the reports concise boolean columns. After that, I trained the model to simply predict the reports and measurements from the raw waveforms themselves, in order to have the pretrained model have weights that reflect indescribable (to humans) patterns that may lie in an ECG. 

The first time I did this, I got a loss (measure of error) that was negative and really high, which didn’t make sense to me. I think it was from not normalizing the machine measurements, and also putting too much “importance” on them in relation to the boolean loss (error). After doing this, I made the pretrained model consider the machine reports a lot more than the measurements when trying to predict and understand ECG structure and patterns, and it is currently still training.

---

# The Next Steps

As I said before, the next steps are to take the pretrained model, and feed the ECGs from the cohort with chest pain (around 40k ECGs) with the LoS label to hopefully get a better prediction on the patient’s length of stay after having an electrocardiogram taken.
