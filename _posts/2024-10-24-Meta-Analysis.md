---
layout: post
title: "Meta-Analysis"
subtitle: "My meta-analysis on an article classifying obstructive and non-obstructive hypertrophic cardiomyopahy"
author: "Arda Altintepe"
---

Over the summer I did research in machine learning to classify two types of cardiomyopathy, dilated and hypertrophic, using electrocardiogram data and machine-learning. Hypertrophic cardiomyopathy also has two forms, when it is obstructive and when it is not, and I found this article which attempted to classify these forms using electrocardiogram data and machine learning-similar to my research over the summer. 

Here is some important background information on the topic: 

**Hypertrophic Cardiomyopathy (HCM)** is a form of heart disease where the **left ventricular wall becomes enlarged**, which can impede blood flow and potentially lead to **heart failure**. HCM can be further classified as either **obstructive** or **non-obstructive**:

- **Obstructive Hypertrophic Cardiomyopathy (HOCM)** is diagnosed when the **Left Ventricular Outflow Gradient (LVOTG)** is greater than **30mmHg**, indicating a blockage in the heart's blood flow that can lead to heart failure and even sudden cardiac death. 
- **Non-Obstructive Hypertrophic Cardiomyopathy (HNCM)** is when the LVOTG is less than 30mmHg. This usually has much a much better prognosis than HOCM, and a large amount of patients can be asymptomatic and never realize they had the condition in the first place.

### **Key Hypotheses of the Study**
- **Null Hypothesis**: HOCM and HNCM cannot be differentiated using only **electrocardiogram (ECG)** data. I found other articles that supported this null hypothesis, including [this paper](https://www.ahajournals.org/doi/pdf/10.1161/01.CIR.58.3.402)
- **Alternative Hypothesis**: HOCM and HNCM can be differentiated by developing a **machine learning model** using ECG features as inputs. This is what the article that my meta-analysis is on attempts to prove.

### **Study Background and Dataset**

The data for this study was collected from the **International Cooperation Center for Hypertrophic Cardiomyopathy** at **Xijing Hospital**. Specifically, it included **172 unique HCM patients** over the course of **five months**. The researchers in the study compared ECG data to **echocardiogram** diagnoses of HOCM and HNCM to identify potential patterns in the ECG data that could predict the type of HCM.

Key points about the dataset:
- **ECGs and corresponding LVOTG gradients** (from echocardiograms) were recorded for each patient.
- Important ECG features like **R-wave** and **S-wave amplitudes** were extracted and tested for **statistical significance** using the **Mann-Whitney U test**. However, I don't think they mention how these features were extracted from the ECGs, which make it very hard to replicate (along with the fact that the data is not public). 
- **Exclusion criteria**: Patients with **postoperative follow-up**, **atrial fibrillation**, **pacemaker implants**, or **bundle branch blocks** were excluded, because conditions could result in abnormal or missing ECG components like the **P-wave** and possibly because they also have their on ECG patterns that would not be analyzeable. 

### **Results and Validation**

- **External Validation**: The model was tested on a separate validation set from the same hospital, resulting in a **C-statistic of 0.776**, indicating a strong predictive ability.
- **Significant Findings**: The **Mann-Whitney U tests** that the authors of the study used revealed that **10 ECG features** had a **p-value** of less than **0.05**, showing significant statistical differences between HOCM and HNCM patients. This contradicts the findings of the previous paper that supported the null hypothesis, which I find very interesting. 
- From what is given in the paper, study provided substantial evidence to **disprove the null hypothesis**, showing that ECG data can indeed differentiate between **HOCM** and **HNCM** through a machine-learning model.

### **Funding and Potential Bias**

The study was funded by multiple government organizations, including:
- The **National Key R&D Program of China** (Grant No. 2018YFA0107400),
- The **Program for Chang-Jiang Scholars** (Grant No. PCSIRT-14R08), 
- The **National Science Funds of China** (Grant No. 82170358).

From my external research, these funding sources above, particularly the **National Key R&D Program**, say that their mission is to spur **innovation** in scientific fields like **biology**, **physics**, and **global change**. All of these sources are government-funded by the chinese government as well, which I think highlights the importance of this study and the importance of finding something novel (in the eyes of the chinese government) For example, the National Science Funds of China is another government-funded organization run by the Ministry of Science and Technology.
 

### **Critical Analysis: Publish or Perish?**

I believe that **publish or perish** pressure definitely could be present in this study, especially considering that
- **Previous research** has consistently supported the **null hypothesis**, making this study’s **novel findings** that go against the null hypothesis interesting and important to innovation and to the government.
- Going with that first point, the study's **government funding** could have definitely pressured the authors to arrive at novel results, maybe causing them to chase a higher metric for the model performance.

On the other hand, the researchers were **hospital-based**, and not affiliated with a university or graduate program where publishing is even more important, at least I think, for academic success in the future. This context could show a lower urgency to publish for career advancement compared to typical academic researchers, since they are already working in the hospital. 


