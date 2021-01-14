# CGATCPred

Files:

1.data

Ch_one.pckl: The fingerprint similarity matrix of chemicals.

Ch_two.pckl: The activities similarity matrix of chemicals.

Ch_three.pckl: The reactions similarity matrix of chemicals.

Ch_four.pckl: The co-occurrence similarity matrix of chemicals.

Ch_five.pckl: The integrated similarity matrix of chemicals.

The above five matrices are all collected from the file "Chemical_chemical.links.detailed.v5.0.tsv.gz" in STITCH database (http://stitch.embl.de/). The row of each matrix represents the similarity value between a compound and all compounds in the benchmark dataset. The range of values is from 0 to 1.

Ch_six.pckl: The similarity matrix calculated by the online program SIMCOMP (http://www.genome.jp/tools/simcomp/). The row of the matrix represents the similarity value between a compound and all compounds in the benchmark dataset. The range of values is from 0 to 1.

Ch_seven.pckl: The chemical similarity matrix calculated by the online program SUBCOMP (http://www.genome.jp/tools/subcomp/). The row of the matrix represents the similarity value between a compound and all compounds in the benchmark dataset. The range of values is from 0 to 1.

Drug_ATC_label.pckl: The benchmark dataset matrix. We employed the benchmark dataset that was first used in Chen et al.’s study (Chen L, Zeng W M, Cai Y D, et al. Predicting anatomical therapeutic chemical (ATC) classification of drugs by integrating chemical-chemical interactions and similarities[J]. PloS one, 2012, 7(4): e35254.). A total of 3,883 drugs, represented by KEGG drug IDs, were obtained. These drugs were classified into 14 groups. The matrix has 3883 rows and 14 columns to store the known drug- ATC code associations.

glove_wordEmbedding.pkl: ATC label word embeddings. We use the 300-dimensional Global Vectors (GloVe) trained on the Wikipedia dataset to represent the information of ATC labels (nodes) in the correlation graph. Each row of the matrix represents the word vector encoding of an ATC code.

If you want to view the value stored in the file, you can run the following command:

```bash
import pickle
import numpy as np
gii = open(‘data’ + '/' + ' Ch_one.pckl', 'rb')
drug_feature_one = pickle.load(gii)
```


2.Code

Extra_label_matrix.py: By entering the compound-ATC label adjacency matrix, the function can calculate the correlation scores between each ATC label.

single_label.py: computing evaluation metric; This function can evaluate the prediction performance of a multi-label classifier. Evaluation indicators are: Hamming loss, Aiming, Coverage, Absolute true rate, Absolute false rate and Accuracy.

network_kfold.py: This function contains the network framework of our entire model and is based on pytorch 1.6. The model includes multiple CNN and GCN layers.

cross_validation.py: This function can test the predictive performance of our model under ten-fold cross-validation.

# Requirements
* python == 3.6
* pytorch == 1.6
* Numpy == 1.16.2
* scikit-learn == 0.21.3

# Require input files

SMSim: The fingerprint similarity matrix of chemicals.

SMExp: The activities similarity matrix of chemicals.

SMDat: The reactions similarity matrix of chemicals.

SMTex: The co-occurrence similarity matrix of chemicals.

SMCom: The integrated similarity matrix of chemicals.

The above five matrices can be collected from the file "Chemical_chemical.links.detailed.v5.0.tsv.gz" in STITCH database.

SMcp: The results of the online program (SIMCOMP). SIMCOMP is used to determine the maximal common substructure of two drugs and calculate the score based on the sizes of the common substructure and two drugs.

SMsub: The results of the online program (SUBCOMP). SUBCOMP is used to determine exactly matching substructures or superstructures, thereby evaluating the similarity score.

Drug_ATC_label: The compound-ATC code adjacency matrix, each row of the matrix corresponds to multiple ATC code labels of a compound.

# Train and test folds
python cross_validation.py --rawdata_dir /Your path --model_dir /Your path --num_epochs Your number --batch_size Your number

rawdata_dir: All input data should be placed in the folder of this path. (The data folder we uploaded contains all the required data.)

model_dir: Define the path to save the model.

num_epochs: Define the maximum number of epochs.

batch_size: Define the number of batch size for training and testing.

All files of Data and Code should be stored in the same folder to run the model.

Example :

```bash
python cross_validation.py --rawdata_dir /data --model_dir /save_model --num_epochs 50 --batch_size 128
```
