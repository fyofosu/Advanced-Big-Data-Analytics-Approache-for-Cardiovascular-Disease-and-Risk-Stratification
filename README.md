# Advanced-Big-Data-Analytics-Approache-for-Cardiovascular-Disease-and-Risk-Stratification
This project uses Apache Spark to accelerate cardiovascular disease (CVD) risk prediction through distributed data processing and machine learning. Using the Framingham Heart Study dataset, it demonstrates how Spark’s cluster computing enhances efficiency, scalability, and model accuracy across VMs.
System Configuration
Virtual Machine Setup

A multi-node Spark cluster was configured using VMs accessed through Michigan Tech’s VCenter:

Master Node: hadoop1

Worker Nodes: hadoop2, with additional nodes configured as needed.

Each VM was assigned a static IP address (e.g., 192.168.13.xxx) and tested for connectivity via ping to ensure smooth Hadoop and Spark communication.

Network Verification

All members verified inter-VM communication:

ping <IP_of_other_VM>


This confirmed that the cluster nodes could exchange data over SSH and network ports.

SSH Configuration

To enable password-less communication between nodes:

ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
scp ~/.ssh/id_rsa.pub hadoop2:~/.ssh/authorized_keys

Apache Spark Cluster Setup

On the master node (hadoop1):

cd /opt
tar czf spark.tar.gz spark
scp spark.tar.gz root@hadoop2:/opt


On the worker node (hadoop2):

cd /opt
tar xvzf spark.tar.gz


Start Spark services:

# Master
/opt/spark/sbin/start-master.sh

# Worker
/opt/spark/sbin/start-worker.sh spark://hadoop1:7077

# Start all nodes
/opt/spark/sbin/start-all.sh


Submit a Spark job:

/opt/spark/bin/spark-submit --master spark://hadoop1:7077 /opt/preprocessing.py

Dataset Information

Source: Framingham Heart Study – Kaggle

Records: 4,000

Features: 15

Target Variable: TenYearCHD (0 = No CHD, 1 = CHD)

Methodology
Preprocessing Method	Model	VM(s) Used	Objective
KNN Imputation	Logistic Regression	1–2	Handle missing data
Z-Score Outlier Removal	Random Forest	3–4	Improve robustness
Min-Max Normalization	Gradient Boosting	5–6	Scale features
Chi-Square Feature Selection	SVM (RBF)	7–8	Enhance feature discrimination
PCA Dimensionality Reduction	Neural Network (MLP)	9–10	Capture hidden patterns

Each team member implemented a unique preprocessing pipeline and machine-learning algorithm on a separate VM pair, demonstrating distributed model training.

Performance Evaluation
Model	Accuracy (%)	ROC-AUC
Logistic Regression	84.67	0.525
Random Forest	84.59	0.518
Gradient Boosting	84.59	0.543
SVM (RBF)	85.06	0.512
Neural Network (MLP)	78.38	0.553
Performance Comparison

Single VM: Longest runtime due to limited resources.

Two VMs: Improved speed via parallel processing.

Multiple VMs: Significantly faster preprocessing and model training.

However, gains plateaued after several nodes due to network coordination overhead.

Challenges

Synchronization and port configuration issues between VMs.

Frequent VM lags and crashes (Firefox resource usage).

Limited connectivity across all 10 VMs.

Memory bottlenecks during Spark actions.

Model tuning difficulties for MLP and SVM convergence.

Conclusion

This distributed Spark project demonstrated the power of parallel computing in accelerating big data analytics for CVD risk prediction. Spark’s in-memory cluster architecture achieved up to 5× faster model execution compared to a single-node setup.

The system is scalable, reproducible, and can serve as a foundation for real-time clinical decision-support systems in healthcare analytics.
