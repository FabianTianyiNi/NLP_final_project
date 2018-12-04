# NLP_final_project
Esta es para CSE 576 proyecto final en ASU
ReadMe File
1. you need to update your configuration enviroment  
  a. This program is done by PyTorch version 0.3.0. CUDA version 9.0. python 3.6 and spaCy 2.0  
  
2. hardware environment  
  a. to finish this project, during the data preprocess, you do not need to use GPU to process. I used 20 CPU (E5-2680v4) to process it. According to my experience, you will need at least 4 or more.  
  
  b. for the training and evaluation part, I used 4 GPU concurrently (Tesla V100-SXM2-16GB) and I trained almost two days to finish it, since the program has to process the full WIKI  
  
3. data preparation  
  a. you need to download Hotpot data (assuming you are using ubuntu)  
    i. the whole training set: wget http://curtis.ml.cmu.edu/datasets/hotpot/hotpot_dev_distractor_v1.json  
    ii. hotpot distract setting: wget http://curtis.ml.cmu.edu/datasets/hotpot/hotpot_dev_fullwiki_v1.json  
    iii. the full wiki   wget http://curtis.ml.cmu.edu/datasets/hotpot/hotpot_train_v1.1.json  
  b. also, you need to download GloVe. In this study, we are using the same dimension as the default amount, which is 300  
  c. as I mentioned, you need to download or update your spaCy to 2.0, otherwise you will have an error, in 1.0 spaCy does not have the property of blank('en')  
4. preprocessing  
  a. you need preprocess your training into distracted setting  
    python main.py --mode prepro --data_file hotpot_train_v1.1.json --para_limit 2250 --data_split train  
    python main.py --mode prepro --data_file hotpot_dev_distractor_v1.json --para_limit 2250 --data_split dev  
  b. for full wiki  
    python main.py --mode prepro --data_file hotpot_dev_fullwiki_v1.json --data_split dev --fullwiki --para_limit 2250  
5. training process  
  python main.py --mode train --para_limit 2250 --batch_size 24 --init_lr 0.1 --keep_prob 1.0 --sp_lambda 1.0  
  note: you can change your batch-size to 12 or less to make your GPU working. Please make sure your GPU and CPU are working concurrently, only CPU or GPU will help you finish the traing process  
    
  We set the checkpoint every 1000 steps as shown down below in the form of HOTPOT-########-######. According to experiment, the whole will converge after 5 epochs and the training loss will stay more or less at 2.00.   
  ![Alt text](/image.png)  
  After the training process, the program will generate a new json file: dev_distractor_pred.json which is useful to to the evaluation  
6. evaluation  
  main.py --mode test --data_split dev --para_limit 2250 --batch_size 24 --init_lr 0.1 --keep_prob 1.0 --sp_lambda 1.0 --save HOTPOT-20180924-160521 --prediction_file dev_distractor_pred.json
