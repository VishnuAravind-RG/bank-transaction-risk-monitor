# Detecting Anomalies in Financial Transactions

This project focuses on detecting anomalous financial transactions using a deep learning approach based on **Autoencoder Neural Networks**. The goal is to identify unusual or suspicious journal entries that may indicate fraud or irregular accounting behavior.

---

## Classification of Financial Anomalies

Financial anomalies can be broadly classified into **two categories** based on how they deviate from normal transaction behavior.

### Global and Local Anomalies (Illustration)

![Global vs Local Anomalies](https://user-images.githubusercontent.com/64821137/231603556-ac7f2d61-14f5-4bc9-804b-859059f4104f.png)

### 1. Global Anomalies

Global anomalies are transactions that contain **rare or unusual individual attribute values**.

**Examples:**
- Extremely high or low posting amounts
- Rarely used posting users or ledger accounts
- Unusual posting times

These are typically detected using **rule-based or red-flag tests** during audits.  
However, such methods often produce **many false positives**, especially for:
- Year-end adjustments  
- Reversal postings  
- Regular bulk provisions  

As a result, global anomaly detection alone is often insufficient.

---

### 2. Local Anomalies

Local anomalies are transactions that involve an **unusual combination of attributes**, even though each attribute individually appears normal.

**Examples:**
- Irregular combinations of posting keys and ledger accounts
- Uncommon profit center usage with valid amounts

Local anomalies are **harder to detect** because fraudulent users try to **mimic normal behavior**.  
These anomalies usually carry a **higher fraud risk**.

---

## Objective of the Project

The objective of this project is to detect both **global and local anomalies** using an **unsupervised deep learning model**.

The approach is based on the following assumptions:
1. Most transactions in an ERP system represent **normal business activity**
2. Fraudulent behavior occurs in **very few transactions**
3. Fraud deviates from normal posting patterns and can be treated as an **anomaly**

By learning what is “normal,” the model can flag transactions that deviate significantly.

---

## Dataset Description

The dataset is designed to resemble real-world **SAP ERP (FICO module)** financial data, similar to the BKPF and BSEG tables.

### Attributes Used

| Attribute | Description |
|--------|-------------|
| BELNR | Accounting document number |
| BUKRS | Company code |
| BSCHL | Posting key |
| HKONT | General ledger account |
| PRCTR | Profit center |
| WAERS | Currency key |
| KTOSL | G/L account key |
| DMBTR | Amount in local currency |
| WRBTR | Amount in document currency |

An additional column called **`label`** is included only for evaluation:
- `regular` → Normal transaction  
- `global` → Global anomaly  
- `local` → Local anomaly  

---

## Autoencoder Neural Networks

An **Autoencoder Neural Network (AENN)** is a type of feed-forward neural network used for **unsupervised learning**.

It learns to:
- Compress the input data into a low-dimensional representation
- Reconstruct the original input from this compressed form

Since the model is trained mainly on **regular transactions**, it struggles to reconstruct anomalous ones.

---

## Autoencoder Architecture

![Autoencoder Architecture](https://user-images.githubusercontent.com/64821137/231604818-312778e7-a652-450f-82a1-ab242fb1a5cf.png)

The network consists of:
- **Encoder**: Compresses input into latent space  
- **Latent Layer**: Low-dimensional representation  
- **Decoder**: Reconstructs the input  

Mathematically:
- Encoder: `z = f(x)`
- Decoder: `x̂ = g(z)`

---

## Reconstruction Error

The **reconstruction error** is the difference between:
- Original input `x`
- Reconstructed output `x̂`

Regular transactions have **low reconstruction error**,  
while anomalous transactions have **high reconstruction error**.

---

## Model Evaluation

After training, reconstruction error is used as an anomaly score.

![Reconstruction Error Plot](https://user-images.githubusercontent.com/64821137/231606709-1c1b4aa9-41f4-4e41-b494-a5b2e0ebfd1a.png)

From the results:
- Regular transactions cluster with low error
- Global anomalies show higher error
- Local anomalies show the highest error

This confirms that the autoencoder effectively separates normal and anomalous transactions.

---

## Conclusion

- Autoencoders can successfully model normal financial behavior
- Reconstruction error is an effective anomaly indicator
- The approach works well for detecting both global and local anomalies

This project serves as a **basic but practical introduction** to anomaly detection in financial systems and can be extended for real-world audit and compliance applications.

---
