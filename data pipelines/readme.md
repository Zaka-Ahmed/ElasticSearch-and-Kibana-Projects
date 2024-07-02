PUT /zaka

POST /zaka/_doc/1
{
  "response": {
    "profile": {
      "resumes": [
        {
          "date": "2021-04-08T03:06:02+00:00",
          "jobID": "1234",
          "resumeID": "e9083748-b953-4e27-91c1-b6e09994d9212df",
          "appID": "11111",
          "link": "https://some-domain/cv/111111/11111", 
          "channel": "",
          "format": ".pdf",
          "source": ""
        }
      ]
    }
  }
}

POST /zaka/_doc/2
{
  "response": {
    "profile": {
      "resumes": [
        {
          "date": "2022-04-08T03:06:02+00:00",
          "jobID": "1235",
          "resumeID": "e9083748-b953-4e27-91c1-b6e09994d9212df",
          "appID": "11111",
          "link": "https://some-domain/cv/111111/11111", 
          "channel": "",
          "format": ".pdf",
          "source": ""
        }
      ]
    }
  }
}



PUT /_ingest/pipeline/zaka_pipeline
{
  "description": "Creates a new field by combining multiple fields",
  "processors": [
    {
      "foreach": {
        "field": "response.profile.resumes",
        "processor": {
          "set": {
            "field": "_ingest._value.link",
            "value": "https://zakaahmed/cv/{{_ingest._value.jobID}}/{{_ingest._value.resumeID}}"
          }
        }
      }
    }
  ]
}




POST /_ingest/pipeline/soumil_pipeline/_simulate
{
  "docs": [
    {
      "_source": {
        "response": {
          "profile": {
            "resumes": [
              {
                "date": "2023-05-08T03:06:02+00:00",
                "jobID": "1237",
                "resumeID": "22222-b953-4e27-91c1-b6e09994d9212df",
                "appID": "11010101",
                "link": "https://some-domain/cv/11111122/111112", 
                "channel": "",
                "format": ".pdf",
                "source": ""
              }
            ]
          }
        }
      }
    }
  ]
}
