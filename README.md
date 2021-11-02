

## Abstract
This replication package includes all scripts that we used in the experiment and
that we can distribute.

This script uses the following packages:
- https://github.com/MKmknd/ILA_Validation
- https://github.com/MKmknd/ILA
- https://github.com/MKmknd/CLA

## Citation

TBD

## How to use

### 0. Motivation example

```
$ cd ./motivation_example/target_repository/
$ bash clone.sh
$ cd ./../
$ bash mk_dir.sh
$ bash conduct.sh
$ python3 evaluate.py
```

The output in ./tables/link_proportion.csv
corresponds to Table 1.

### 1. Prepare the data

Clone all the studied projects.

```
$ cd ./prepare_data/repository
$ git clone xxx
```

Extract all issues from the issue tracking system

```
$ cd ./../extract_issues
$ vim extract_issues_from_JIRA.py # please modify PROJECT_NAME_LIST for your studied projects (e.g., AVRO)
$ vim parse_issue_list_json.py # please modify PROJECT_NAME_LIST for your studied projects (e.g., AVRO)
$ vim extract_data_from_JIRA.py # please modify PROJECT_NAME_LIST for your studied projects (e.g., AVRO)
$ vim check_all_log_repository.py # please modify project_name_list for your studied projects (e.g., avro)
$ vim extract_log_msg_repository.py # please modify project_name_list for your studied projects (e.g., avro)
$ bash mk_dir.sh
$ python3 extract_issues_from_JIRA.py
$ python3 parse_issue_list_json.py 
$ python3 extract_data_from_JIRA.py
$ python3 check_all_log_repository.py
$ python3 extract_log_msg_repository.py
```


### 2. Execute RQ1 (ILAs)

Please modify "your project here" to your studied projects for all scripts.

You need to clone the following repository in ./RQ1/8/:
https://github.com/MKmknd/bouffier-java
And please build the image:

```
docker build . -t m-kondo/bouffier-java:latest
```

Note that the base image "gradle:5.4.1-jdk8" is currently expired.
Please update this base image (we used this image.)


```
$ cd ./../../RQ1
$ bash mk_dir.sh
$ cd 1/
$ python3 exe_keyword_extraction.py
$ cd ./../random_delete/
$ python3 generate_delete_data.py
$ python3 ground_truth.py
$ cd ./../3/
$ python3 exe_time_filtering.py
$ cd ./../5/
$ bash conduct.sh
$ cd ./../7/
$ bash conduct.sh
$ cd ./../8
$ vim extract_AST.py # please modify {{absolute path}} following your environment
$ bash para.sh
$ python3 exe_comment.py
$ cd ./../9
$ bash conduct_loner.sh
$ bash conduct_phantom.sh
$ cd ./../10
$ bash conduct.sh
$ cd ./../11
$ bash conduct.sh
$ cd ./../14
$ bash conduct.sh
$ cd ./../evaluation
$ python3 ILA.py
$ python3 evaluation_with_restriction.py
$ python3 SKESD.py
$ python3 make_table_with_rank.py
```


The output in /RQ1/evaluation/tables/*
corresponds to Table 7 in our paper.
 

### 3. Execute RQ2 (Defect Prediction)

First you need to use cregit to prepare the view repo in
/prepare_data/repository_cregit.
The details can be found in our paper.

Second, to prepare the change metrics, please
generate the csv file for the studied project with Commit Guru (http://commit.guru/)
and store the csv file in /RQ2/commitguru_data/.
The original paper is:
C.Rosen, B.Grawi, E. Shihab, Commit Guru: Analytics and Risk Prediction of Software Commits, In FSE'15

Also, please replace "project name here" with your studied project name.

Finally, you need to prepare the SMOTUNED approach.
For example, the original script of SMOTUNED can be available in the following GitHub repository:
https://github.com/ai-se/Smote_tune
The original paper is:
Agrawal, A., Menzies, T.: Is “better data” better than “better data miners”? In: Proceedings of the 40th International Conference on Software Engineering (ICSE), pp. 1050–1061. IEEE (2018)

The SMOTUNED approach should be implemented as the ```smote_tuned``` method in the following script:
/RQ2/DP/prediction.py



```
$ cd ./../../RQ2
$ bash mk_dir.sh
$ cd CLA
$ python3 extract_all_commits.py
$ bash conduct.sh
$ python3 make_bootstrap_db.py
$ cd ./../commitguru_data
$ python3 make_db.py
$ cd ./../DP
$ python3 create_author_date.py
$ bash execute.sh
$ cd ./../evaluation
$ python3 diff_evaluation.py
$ python3 diff_make_table.py
```

The output in /RQ2/evaluation/tables/diff_rank_avro_50_LR.csv
corresponds to Table 8.
The output in /RQ2/evaluation/tables/diff_rank_sum.csv
corresponds to Table 9.
The output in /RQ2/evaluation/tables/diff_rank_LR_TS,PH,RF,SVM.csv
corresponds to Table 10.


### 4. Execute discussion 

Please replace "project name here" with your studied project name.
 
```
$ cd ./../../discussion
$ bash mk_dir.sh
$ cd ./RF
$ python3 analyze_newlink.py
$ cd ./../release
$ python3 parse_issue_list_json.py
$ python3 show_distribution_with_time_filtering.py
$ python3 show_distribution_for_paper.py
$ cd ./../time_interval
$ python3 ILA_diff_thresholds.py
$ python3 evaluation_diff_thresholds.py
$ python3 SKESD_diff_thresholds.py
$ python3 make_table_diff_thresholds.py
$ cd ./../effort_aware
$ python3 effort_diff_evaluation.py
$ python3 effort_diff_make_table.py
```

The output in /discussion/RF/result/*
corresponds to Table 11 (need to manually check all of them)
The output in /discussion/release/plot/*
corresponds to Figure 6
The output in /discussion/time_interval/tables_th/*
corresponds to Table 12
The output in /discussion/effort_aware/effort_tables/*
corresponds to Table 13


## Additional results

./additinal_results
This result includes additional results.

./additional_results/resampling_comparisons/
This directory includes the results of all comparisons across the resampling
approaches (i.e., SMOTUNED, the under sampling, and MAHAKIL).