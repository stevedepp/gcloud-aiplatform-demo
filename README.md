# gcloud-aiplatform-demo
Demonstrates Google ai-platform CLI for multi-class classification as shown in Geewax chapter 18



https://cloud.google.com/sdk/gcloud/reference/services/enable

[x] step 1: Enable the API
  - `gcloud services list`
  - `gcloud services enable ml.googleapis.com`
<img width="682" alt="1" src="https://user-images.githubusercontent.com/38410965/97049962-9070b200-154a-11eb-94ae-1ea8df988f14.png">
Note: 
ml-engine replaced by gcloud ai-platform

https://cloud.google.com/sdk/gcloud/reference/ai-platform/models/create

[x] step 2:  create model
  - `gcloud ai-platform models create census --description 'Census example'`
  - `gcloud ai-platform models list`
  - `gcloud ai-platform versions list --model census`
<img width="682" alt="2" src="https://user-images.githubusercontent.com/38410965/97049979-95356600-154a-11eb-891e-4a25003f485d.png">

https://github.com/amygdala/tensorflow-workshop/tree/master/workshop_sections/wide_n_deep

[x] step 3: set up the environment 
  - `python3 -m venv .venv` 
  - `source .venv/bin/activate` 
  - `mkdir data`
[x] step 4: retrieve the data
  - `git clone https://github.com/amygdala/tensorflow-workshop.git`
  - `ls tensorflow-workshop/workshop_sections/wide_n_deep`
  - `cp tensorflow-workshop/workshop_sections/wide_n_deep/adult* ./data/`
<img width="682" alt="3" src="https://user-images.githubusercontent.com/38410965/97050001-99fa1a00-154a-11eb-8247-d9654f86f2fd.png">

https://cloud.google.com/storage/docs/gsutil/commands/mb

[x] step 5: move data to GCP google storage 
  - `gsutil cp ./data/adult.test.csv data gs://dv-auto-ml-depp/data`
  - `gsutil cp ./data/adult.data.csv data gs://dv-auto-ml-depp/data`
  - `gsutil ls gs://dv-auto-ml-depp/data`
<img width="682" alt="4" src="https://user-images.githubusercontent.com/38410965/97050011-9e263780-154a-11eb-9267-dfd7b0930f51.png">

https://github.com/GoogleCloudPlatform/cloudml-samples

[x] step 6: retrieve the canned model
  - `git clone https://github.com/GoogleCloudPlatform/cloudml-samples`
  - `ls cloudml-samples/census/tensorflowcore/trainer`
  - `cd cloudml-samples/census/tensorflowcore`
<img width="682" alt="5" src="https://user-images.githubusercontent.com/38410965/97050028-a67e7280-154a-11eb-829a-123e0dc524f8.png">

https://cloud.google.com/sdk/gcloud/reference/ai-platform/jobs/submit/training

https://cloud.google.com/ai-platform/prediction/docs/runtime-version-list

[x] step 7: submit the job
  - 'gcloud ai-platform jobs submit training cloud1 --stream-logs --runtime-version 1.15 --job-dir gs://dv-auto-ml-depp/census --module-name trainer.task --package-path trainer/ --region us-central1 -- --train-files gs://dv-auto-ml-depp/data/adult.data.csv --eval-files gs://dv-auto-ml-depp/data/adult.test.csv --train-steps 10000 --eval-steps 500'
<img width="682" alt="6" src="https://user-images.githubusercontent.com/38410965/97050044-ada58080-154a-11eb-9571-301e3bbc2245.png">


<img width="1151" alt="7" src="https://user-images.githubusercontent.com/38410965/97050052-b302cb00-154a-11eb-8b03-66f7a3a6ffb2.png">


<img width="1208" alt="8" src="https://user-images.githubusercontent.com/38410965/97050061-b72ee880-154a-11eb-93ef-935641269477.png">


<img width="682" alt="9" src="https://user-images.githubusercontent.com/38410965/97050080-bdbd6000-154a-11eb-830f-d24c6e940878.png">


<img width="773" alt="10" src="https://user-images.githubusercontent.com/38410965/97050106-c57d0480-154a-11eb-82bf-1873b006e749.png">


<img width="773" alt="11" src="https://user-images.githubusercontent.com/38410965/97050116-cada4f00-154a-11eb-8b0e-4b624aa81c96.png">


<img width="682" alt="12" src="https://user-images.githubusercontent.com/38410965/97050127-cf066c80-154a-11eb-8088-caad85ac9a77.png">


<img width="682" alt="13" src="https://user-images.githubusercontent.com/38410965/97050129-d299f380-154a-11eb-8aa6-6aa3eaad26cf.png">


<img width="682" alt="14" src="https://user-images.githubusercontent.com/38410965/97050135-d6c61100-154a-11eb-9d6e-fb9da9e28d69.png">


<img width="682" alt="15" src="https://user-images.githubusercontent.com/38410965/97050139-dc235b80-154a-11eb-871a-92232d713649.png">


<img width="682" alt="16" src="https://user-images.githubusercontent.com/38410965/97050152-dfb6e280-154a-11eb-8c4e-a59bb87ed3a1.png">


<img width="682" alt="17" src="https://user-images.githubusercontent.com/38410965/97050174-e6455a00-154a-11eb-9211-c7243ed8716e.png">


<img width="1208" alt="18" src="https://user-images.githubusercontent.com/38410965/97050187-ec3b3b00-154a-11eb-843c-e79a7c082ad7.png">


<img width="1208" alt="19" src="https://user-images.githubusercontent.com/38410965/97050208-f52c0c80-154a-11eb-8bef-50811e89beb1.png">


<img width="687" alt="20" src="https://user-images.githubusercontent.com/38410965/97050217-f9f0c080-154a-11eb-94d9-d06a797f28ee.png">


<img width="682" alt="21" src="https://user-images.githubusercontent.com/38410965/97050223-fe1cde00-154a-11eb-929d-8a9fe437b79b.png">


<img width="682" alt="22" src="https://user-images.githubusercontent.com/38410965/97050236-037a2880-154b-11eb-812a-a1cd7199778c.png">


<img width="682" alt="23" src="https://user-images.githubusercontent.com/38410965/97050247-070daf80-154b-11eb-8a89-e8b946afd653.png">


<img width="682" alt="24" src="https://user-images.githubusercontent.com/38410965/97050256-0aa13680-154b-11eb-8989-87dfbb22055f.png">


<img width="682" alt="25" src="https://user-images.githubusercontent.com/38410965/97050269-0f65ea80-154b-11eb-91dd-c24d20392bea.png">




