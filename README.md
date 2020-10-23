### Demonstrates Google ai-platform CLI for multi-class classification as shown in Geewax chapter 18

Geewax, J. J. (2018). Google Cloud Platform in Action. Manning Publications.

My demo video above and here are the 16 steps: 

[![](http://img.youtube.com/vi/Wp7F7zj4vVo/0.jpg)](http://www.youtube.com/watch?v=Wp7F7zj4vVo "Demo of GCP ai-platform API use via their command line interface")


https://cloud.google.com/sdk/gcloud/reference/services/enable

- [x] step 1: Enable the API
  - `gcloud services list`
  - `gcloud services enable ml.googleapis.com`
<img width="682" alt="1" src="https://user-images.githubusercontent.com/38410965/97049962-9070b200-154a-11eb-94ae-1ea8df988f14.png">
Note: 
ml-engine replaced by gcloud ai-platform

https://cloud.google.com/sdk/gcloud/reference/ai-platform/models/create

- [x] step 2:  create model
  - `gcloud ai-platform models create census --description 'Census example'`
  - `gcloud ai-platform models list`
  - `gcloud ai-platform versions list --model census`
<img width="682" alt="2" src="https://user-images.githubusercontent.com/38410965/97049979-95356600-154a-11eb-891e-4a25003f485d.png">

https://github.com/amygdala/tensorflow-workshop/tree/master/workshop_sections/wide_n_deep

- [x] step 3: set up the environment 
  - `python3 -m venv .venv` 
  - `source .venv/bin/activate` 
  - `mkdir data`
- [x] step 4: retrieve the data
  - `git clone https://github.com/amygdala/tensorflow-workshop.git`
  - `ls tensorflow-workshop/workshop_sections/wide_n_deep`
  - `cp tensorflow-workshop/workshop_sections/wide_n_deep/adult* ./data/`
<img width="682" alt="3" src="https://user-images.githubusercontent.com/38410965/97050001-99fa1a00-154a-11eb-8247-d9654f86f2fd.png">

https://cloud.google.com/storage/docs/gsutil/commands/mb

- [x] step 5: move data to GCP google storage 
  - `gsutil cp ./data/adult.test.csv data gs://dv-auto-ml-depp/data`
  - `gsutil cp ./data/adult.data.csv data gs://dv-auto-ml-depp/data`
  - `gsutil ls gs://dv-auto-ml-depp/data`
<img width="682" alt="4" src="https://user-images.githubusercontent.com/38410965/97050011-9e263780-154a-11eb-9267-dfd7b0930f51.png">

https://github.com/GoogleCloudPlatform/cloudml-samples

- [x] step 6: retrieve the canned model
  - `git clone https://github.com/GoogleCloudPlatform/cloudml-samples`
  - `ls cloudml-samples/census/tensorflowcore/trainer`
  - `cd cloudml-samples/census/tensorflowcore`
<img width="682" alt="5" src="https://user-images.githubusercontent.com/38410965/97050028-a67e7280-154a-11eb-829a-123e0dc524f8.png">

https://cloud.google.com/sdk/gcloud/reference/ai-platform/jobs/submit/training

https://cloud.google.com/ai-platform/prediction/docs/runtime-version-list

- [x] step 7: submit the job
  - `gcloud ai-platform jobs submit training cloud1 --stream-logs --runtime-version 1.15 --job-dir gs://dv-auto-ml-depp/census --module-name trainer.task --package-path trainer/ --region us-central1 -- --train-files gs://dv-auto-ml-depp/data/adult.data.csv --eval-files gs://dv-auto-ml-depp/data/adult.test.csv --train-steps 10000 --eval-steps 500`
<img width="682" alt="6" src="https://user-images.githubusercontent.com/38410965/97050044-ada58080-154a-11eb-9571-301e3bbc2245.png">

- [x] step 8: read the logs
    - took 30 minutes to get log results
     - notes old version of some dependencies
     - python 2.7 end of life at google Jan 2020, but still seems to run
   - global steps / sec = 4.65 and with 10,000 steps / 4.65 / 60 = 35 minutes. 
<img width="1151" alt="7" src="https://user-images.githubusercontent.com/38410965/97050052-b302cb00-154a-11eb-8b03-66f7a3a6ffb2.png">

- [x] We can also see logs on the GCP
https://console.cloud.google.com/logs/query;query=resource.labels.job_id%3D%22cloud1%22%20timestamp%3E%3D%222020-10-23T07:43:09Z%22?project=msds434dv6
<img width="1208" alt="8" src="https://user-images.githubusercontent.com/38410965/97050061-b72ee880-154a-11eb-93ef-935641269477.png">

- [x] step 9: review the job
  - `gcloud ai-platform jobs describe cloud1`  
<img width="682" alt="9" src="https://user-images.githubusercontent.com/38410965/97050080-bdbd6000-154a-11eb-830f-d24c6e940878.png">

- [x] step 10: review the output files and model stored in the bucket
  - `gsutil ls gs://dv-auto-ml-depp/census`
  - `gsutil ls gs://dv-auto-ml-depp/census/export`
<img width="773" alt="10" src="https://user-images.githubusercontent.com/38410965/97050106-c57d0480-154a-11eb-82bf-1873b006e749.png">

https://cloud.google.com/sdk/gcloud/reference/ai-platform/versions/create

- [x] step 11: create version 1 of the model
  - `gcloud ai-platform versions create v1 --model census --staging-bucket gs://dv-auto-ml-depp --origin gs://dv-auto-ml-depp/census/export --runtime-version 1.15`

- [x] note for step 11: the documentation is a bit sketchy here. 
  - `version_name` is the only mandatory argument and `--model` the only mandatory flag, but errors are thrown unless you include these flags: `--staging_bucket`, `--origin`, `--runtime-version` 
  
    https://cloud.google.com/sdk/gcloud/reference/ai-platform/versions/create#--model

    https://cloud.google.com/sdk/gcloud/reference/ai-platform/versions/create#--staging-bucket

    https://cloud.google.com/sdk/gcloud/reference/ai-platform/versions/create#--origin

    https://cloud.google.com/sdk/gcloud/reference/ai-platform/versions/create#--runtime-version
<img width="773" alt="11" src="https://user-images.githubusercontent.com/38410965/97050116-cada4f00-154a-11eb-8b0e-4b624aa81c96.png">

https://cloud.google.com/sdk/gcloud/reference/ai-platform/predict

- [x] step 12: locate a `test.json` file to make a single online predictions from version 1 of the model
  - `ls ../test.*`
  - `gcloud ai-platform predict --model census --version v1 --json-instances ../test.json`
—> 78.4% confidence the correct class is “<=50k” 
<img width="682" alt="12" src="https://user-images.githubusercontent.com/38410965/97050127-cf066c80-154a-11eb-8088-caad85ac9a77.png">

  `cat test.json`

<img width="682" alt="13" src="https://user-images.githubusercontent.com/38410965/97050129-d299f380-154a-11eb-8aa6-6aa3eaad26cf.png">

- [x] step 13: copy the `test.json` file into `test2.json` and see model sensitivity with a change the age feature modified from 25 to 20.
  - `ls ../test*`
  - `gcloud ai-platform predict --model census --version v1 --json-instances ../test2.json`
—> 84.7% confidence the correct class is “<=50k” 

<img width="682" alt="14" src="https://user-images.githubusercontent.com/38410965/97050135-d6c61100-154a-11eb-9d6e-fb9da9e28d69.png">

  `cat test2.json`

<img width="682" alt="15" src="https://user-images.githubusercontent.com/38410965/97050139-dc235b80-154a-11eb-871a-92232d713649.png">

https://cloud.google.com/sdk/gcloud/reference/ai-platform/jobs/submit/prediction

- [x] step 14: submit prediction job for a test_batch.json dataset with 11 rows for ages 20-70 
  - `gcloud ai-platform jobs submit prediction prediction1 --model census --version v1 --data-format text --region us-central1 --input-paths gs://dv-auto-ml-depp/test_batch.json --output-path gs://dv-auto-ml-depp/prediction1-output`
  - (somewhat disconcerting to have no message when QUEUED for > 5 minutes)
<img width="682" alt="16" src="https://user-images.githubusercontent.com/38410965/97050152-dfb6e280-154a-11eb-8c4e-a59bb87ed3a1.png">

  `nano test_batch.json`

<img width="682" alt="17" src="https://user-images.githubusercontent.com/38410965/97050174-e6455a00-154a-11eb-9211-c7243ed8716e.png">

https://console.cloud.google.com/logs/query;query=resource.labels.job_id%3D%22prediction1%22%20timestamp%3E%3D%222020-10-23T09:43:55Z%22?project=msds434dv6

- [x] step 14 continued: Look at the GCP logs for this job
<img width="1208" alt="18" src="https://user-images.githubusercontent.com/38410965/97050187-ec3b3b00-154a-11eb-843c-e79a7c082ad7.png">

- [x] step 14 continued: Look at the GCP logs for this job
<img width="1208" alt="19" src="https://user-images.githubusercontent.com/38410965/97050208-f52c0c80-154a-11eb-8bef-50811e89beb1.png">
  <img width="687" alt="20" src="https://user-images.githubusercontent.com/38410965/97050217-f9f0c080-154a-11eb-94d9-d06a797f28ee.png">

- [x] step15: review the prediction job output in the bucket and the description of the job
  - `gsutil ls gs://dv-auto-ml-depp/prediction1-output`
  - `gsutil cat gs://dv-auto-ml-depp/prediction1-output/prediction.results-00000-of-00001`
  - `gcloud ai-platform jobs describe prediction1`

<img width="682" alt="21" src="https://user-images.githubusercontent.com/38410965/97050223-fe1cde00-154a-11eb-929d-8a9fe437b79b.png">
<img width="682" alt="22" src="https://user-images.githubusercontent.com/38410965/97050236-037a2880-154b-11eb-812a-a1cd7199778c.png">

- [x] note: gcloud ai-platform jobs submit prediction …
  - again the documentation is a bit confusing: on the one hand `--runtime_version` “must be specified unless `--master-image-uri` is specified” and in the error “Runtime version should only be specified when the model URI is suppled”
https://cloud.google.com/sdk/gcloud/reference/ai-platform/jobs/submit/prediction#--data-format
<img width="682" alt="23" src="https://user-images.githubusercontent.com/38410965/97050247-070daf80-154b-11eb-8a89-e8b946afd653.png">
<img width="734" alt="23 1" src="https://user-images.githubusercontent.com/38410965/97053016-1b07e000-1550-11eb-9f12-39ab59c0e877.png">

https://cloud.google.com/storage/docs/deleting-buckets

- [x] step 16: don’t over pay: delete the bucket
  - `gsutil rm -r gs://dv-aut-ml-depp`
  - alternatives:
        - `rm` will remove bucket and any contents in one go
        - `rb` will removes bucket only if empty

<img width="682" alt="24" src="https://user-images.githubusercontent.com/38410965/97050256-0aa13680-154b-11eb-8989-87dfbb22055f.png">

https://cloud.google.com/sdk/gcloud/reference/ml-engine/versions/delete

- [x] step 16 continued: delete version then delete model then disable api
  - `gcloud ai-platform versions delete v1 --model census`
  - `gcloud ai-platform models delete census`
  - `gcloud services disable ml.googleapis.com`

<img width="682" alt="25" src="https://user-images.githubusercontent.com/38410965/97050269-0f65ea80-154b-11eb-91dd-c24d20392bea.png">




