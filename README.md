# DermaTriage AI

**Track:** AI + Health (4.1.4 Your Own Idea)
**Team:** Leen Bakri (Solo Entry)

## The problem
Many patients—especially those in underserved areas or facing long specialist waitlists—delay getting suspicious skin spots checked because they cannot easily tell whether a lesion needs urgent attention. Everyday patients need an accessible educational triage tool to help them understand visual skin patterns and decide when to prioritize a dermatologist appointment. *(Disclaimer: This prototype is for educational purposes only and does not provide medical diagnoses.)*

## What we built
DermaTriage AI is an interactive web application where users upload a single close-up photo of a skin lesion and answer two clinical ABCDE symptom questions (whether the spot is evolving in size/color or bleeding/itching). Rather than relying solely on an image label, the app combines computer vision with the user's reported symptoms to output either a **Routine Monitor** or **Priority Dermatologist Visit** recommendation.

## How it works
* **Data:** Used the public **DermaMNIST** dataset (MedMNIST v2), containing 28×28 RGB dermatoscopic images across 7 skin lesion classes. Kept the official training split (7,007 images) and held-out test split (2,005 images) strictly separate to prevent data leakage.
* **Model & Custom Safety Twist:** Built in **Python** on **Google Colab** using a **Random Forest Classifier** (100 decision trees) benchmarked against a majority-class `DummyClassifier` baseline, wrapped in a **Gradio** interface with a clinical ABCDE symptom override rule.

## Results
* **Baseline vs. Model:** Our majority-class baseline scored **66.88% accuracy** on unseen test images, while our Random Forest model achieved **71.87% accuracy** (+4.99%).
* **Missed Cases vs. False Alarms (Honesty & Safety Check):** Evaluating high-risk lesions (`melanoma`, `basal cell carcinoma`, and `actinic keratoses`) on the 2,005 test images showed **94 high-risk lesions caught**, **298 missed high-risk cases (false negatives)**, and **58 false alarms (false positives)**. Because common moles make up ~67% of the dataset, the image-only model frequently misclassifies high-risk spots as benign moles—proving why our manual ABCDE symptom checkboxes are critical as a human-in-the-loop safety net.
* **Skin Tone & Image Limitations:** The model struggles on blurry phone photos and darker skin tones due to underrepresentation in `DermaMNIST`.

![Test Set Confusion Matrix](confusion_matrix.png)

## What we'd do next
* Train on balanced, diverse skin-tone datasets (such as Fitzpatrick17k) to reduce demographic bias across lighter and darker skin tones.
* Upgrade to higher-resolution images and a Convolutional Neural Network (CNN) with Grad-CAM heatmaps to lower the false-negative rate.

## Sources
* **Dataset:** MedMNIST v2 / DermaMNIST (Yang et al., 2023) — https://medmnist.com
* **Libraries & Tools:** Python, Google Colab, scikit-learn, Gradio, Matplotlib, Pillow.
* **AI Assistance:** Used Google Gemini as a coding and debugging assistant to structure the Python/Gradio starter pipeline and evaluate error metrics.
