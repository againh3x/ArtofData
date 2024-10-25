---
layout: post
title: "Meta-Analysis"
subtitle: "My meta-analysis on an article classifying obstructive and non-obstructive hypertrophic cardiomyopahy"
author: "Arda Altintepe"
---

Over the summer I did research in machine learning to classify two types of cardiomyopathy, dilated and hypertrophic, using electrocardiogram data and machine-learning. Hypertrophic cardiomyopathy also has two forms, when it is obstructive and when it is not, and I found this article which attempted to classify these forms using electrocardiogram data and machine learning-similar to my research over the summer. 

Here is some important background information on the topic: 

**Hypertrophic Cardiomyopathy (HCM)** is a form of heart disease where the left ventricular wall becomes enlarged, which can impede blood flow and potentially lead to heart failure. HCM can be further classified as either **obstructive** or **non-obstructive**:

- **Obstructive Hypertrophic Cardiomyopathy (HOCM)** is diagnosed when the Left Ventricular Outflow Gradient (LVOTG) is greater than 30mmHg, indicating a blockage in the heart's blood flow that can lead to heart failure and even sudden cardiac death. 
- **Non-Obstructive Hypertrophic Cardiomyopathy (HNCM)** is when the LVOTG is less than 30mmHg. This usually has much a much better prognosis than HOCM, and a large amount of patients can be asymptomatic and never realize they had the condition in the first place.

### **What were the Null and Alternative Hypotheses of the Study?**
- **Null Hypothesis**: HOCM and HNCM cannot be differentiated using only **electrocardiogram (ECG)** data. I found other articles that supported this null hypothesis, including [this paper](https://www.ahajournals.org/doi/pdf/10.1161/01.CIR.58.3.402)
- **Alternative Hypothesis**: HOCM and HNCM can be differentiated by developing a **machine learning model** using ECG features as inputs. This is what the article that my meta-analysis is on attempts to prove.

### **Where did they get the data from? Who were the researchers?**

The data for this study was collected from the **International Cooperation Center for Hypertrophic Cardiomyopathy** at **Xijing Hospital**. Specifically, it included **172 unique HCM patients** over the course of **five months**. The researchers in the study compared ECG data to **echocardiogram** diagnoses of HOCM and HNCM to identify potential patterns in the ECG data that could predict the type of HCM.

The people who collected this data all worked at the Department of Cardiology, Xijing Hospital, the Fourth Military Medical University, Xi’an, Shaanxi, China. The dataset that they used is also not available for public use, meaning that the study could not be replicated at home by someone like me who doesn't work at the Xijing Hospital.

The authors were interested in this data because it allowed them to compare the electrocardiograms to the echocardiogram diagnosis for obstructive or non-obstructive hypertrophic cardiomyopathy. This allows them to attempt to find any patterns with any ECG features and ultimately create a model with the inputs with the most significant feature importance.

- Specifically, **ECGs and corresponding LVOTG gradients** (from echocardiograms) were recorded for each patient. Remember that the gradient determines if the condition is obstructive or not.
  
- Electrocardiogram features were extracted and tested for **statistical significance** accross both patient populations using the **Mann-Whitney U test**. However, I don't think they mention how these features were extracted from the ECGs, which make it very hard to replicate (along with the fact that the data is not public). 
- **Exclusion criteria**: Patients with **postoperative follow-up**, **atrial fibrillation**, **pacemaker implants**, **bundle branch blocks**, and what they called **missing data** were excluded; I believe because these conditions could result in abnormal  ECG components like not having **P-waves** and possibly because they also have their on ECG patterns that would not be analyzeable. 

- **External Validation**: The model was tested on a separate validation set from the same hospital, resulting in a **C-statistic of 0.776**, which indicates a strong predictive ability. Them using an external validation set adds a lot of credibility to their paper, since an external validation set is essentially just a dataset of new patients admitted to the hospital, where they can get an accurate sense of the strength of their model in differentiating the two conditions. The paper would be a lot less credible if they didn't add this part about an external validation set. They also provide the full equation for their logistic regression model, with the constant and the coefficient weights, which was helpful. However, from what I could find, neither their code or data is available to the public. Again, this makes it impossible to reproduce.

Here is an image about the Associated Data section, which is essentially blank other than the supplemental images and tables that were featured in the article:
 
![Data Availability Image]({{ '/assets/img/screenshot.png' | relative_url }})

- **Significant Findings**: The **Mann-Whitney U tests** that the authors of the study used showed that **10 ECG features** had a **p-value** of less than **0.05**, showing significant statistical differences between HOCM and HNCM patients. This contradicts the findings of the previous paper that supported the null hypothesis, which I find very interesting. 
- From what is given in the paper, study provided substantial evidence to **disprove the null hypothesis**, showing that ECG data can indeed differentiate between **HOCM** and **HNCM** through a machine-learning model.

### **Funding and Potential Bias**

The study was funded by multiple government organizations, including:
- The **National Key R&D Program of China** (Grant No. 2018YFA0107400),
- The **Program for Chang-Jiang Scholars** (Grant No. PCSIRT-14R08), 
- The **National Science Funds of China** (Grant No. 82170358).

From my external research, these funding sources above, particularly the **National Key R&D Program**, say that their mission is to spur **innovation** in scientific fields like **biology**, **physics**, and **global change**. All of these sources are government-funded by the chinese government as well, which I think highlights the importance of this study and the importance of finding something novel (in the eyes of the chinese government) For example, the National Science Funds of China is another government-funded organization run by the Ministry of Science and Technology. I'm not really sure what bias could be involved other than really focusing on finding something novel. 
 

### **Critical Analysis: Publish or Perish?**

I believe that **publish or perish** pressure definitely could be present in this study, especially considering that
- **Previous research** has consistently supported the **null hypothesis**, making this study’s **novel findings** that go against the null hypothesis interesting and important to innovation and to the government.
- Going with that first point, the study's **government funding** could have definitely pressured the authors to arrive at novel results, maybe causing them to chase a higher metric for the model performance. Also, because negative studies supporting the null hypothesis already exist, this paper would be essentially useless (not useless of course just a lot less impactful) if they did not find any link between ECG data and HCM being obstructive or not. Therefore I think that combined with government funding, there was definitely a lot of presssure on the authors to prove the alternative hypothesis. 

On the other hand, the researchers were **hospital-based**, and not affiliated with a university or graduate program where publishing is even more important, at least I think, for academic success in the future. This context could show a lower urgency to publish for career advancement compared to typical academic researchers, since they are already working in the hospital. 

Also, the paper stated that there were no conflicts of interest to declare.

### **Conclusion**

Overall, I thought this paper brought up some really interesting and novel results relating to my research over the summer. I think that they backed up their data well with an external validation dataset and didn't have anything "suspicious" or "invalid" other than just being novel. It's also indexed by pubMED which I believe adds credibility as well. However, I definitely think publish or perish was present, and I found it interesting to read a paper while looking through the lense of the authors who were facing the pressure of publication. 


