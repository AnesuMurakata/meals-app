# Fullstack Engineer Case Study Excecutive Summary.

This document presents a structured solution to the fullstack engineer case study, addressing challenges related to HubSpot form failures, page view analytics, system failures and system feature enhancement.

## * Diagnosing& Solving HubSpot Form Failures.

#### *  Root Cause Analysis

To identify the cause of the intermetent 400 errors, a structured debugging approach which involves logging, validation, error pattern analysis will be followed.

1. Next JS Validation
   Ensure the frontend application validates all required fields and correctly handles the user inputs ensuring they follow the HubSpot API schema.
2. Backend Validation
   Ensure the backend api validates the request from the frontend ensuring it follows correct HubSpot API schema.
3. HubSpot Logs
   Check if all requets are reaching the HubSpot API by monitoring their API logs, validate the failed requests and check the errors associated with the requests. Check for rate limits.
4. Logging
   implement detailed logging, capture full requests/response cycles including payloads and headers for monitoring and analysis. Ensure to clear old logs to avoid keeping unnecessary data.

#### Mitigation Plan

1. Implement robust validation on the backend and on the frontend to prevent malformed requests.
2. Provide user feedback on submission failures and guide users on corrective actions
3. Implement a retry mechanism by storing failed requests in a queue and retry asynchronoulsy with exponential backoff
4. Track failed submssions for manual reviews

#### High Level Diagram

[![](https://mermaid.ink/img/pako:eNqVVE1v1DAQ_SuWT1tpCxXiFKFIQFnx0S1lAxeUy2w8m3W7scPYLq2q_veO891mJUQOiZ15nvfejO0HWViFMpEO_wQ0BZ5rKAmq3Ah-aiCvC12D8eKXQ5r_vcQ7L8A131fXTqzIGo9GzZHn12BKG7Hd6AMUN0eRF7YcwTwptSlFhnSrCxSLNnQyX_YjYMC4ZIOe7rvpYoNKuyPoz2Gb1bbR3g_fX31pce07Gj5N0-gsEVnYVtqLlaWuNPE3R1s1SQz8BVLMzWV0fpqnhTB28JVEV5HuOXwAdoIG1gsENc0IB8-hokDnduEgPoLDNhCfbvHpRNybszPx_dsI6Ygm5ppcrMbV1vTJ8OBQrEAfUP2b4i1TfCKyNGNJ06YTzOItDQmfGT-m6SdWtSXgRvbqFm1fs2KPKnCOk3HxpfUo7C2S6PXEzVWSDUaJr3bbbAmN7t2WXqedgqa2zmk23Pk1syK3jGss9mC0q0bCxtLEfidtSHmkCkNTucov2_p_fRsqusGKTYsd2apVMGncGu561_yFWLSX8udb8gqpAoN8PGKRAuGkMnIpqxjWim-LhxjIpd9jhblMeKiAbnKZm0fGQfA2uzeFTDwFXEruQ7mXyQ5Y2FKGWoHv75nhLx_K39aOcz62vF_W7eXU3FGPT6r0g6s?type=png)](https://mermaid.live/edit#pako:eNqVVE1v1DAQ_SuWT1tpCxXiFKFIQFnx0S1lAxeUy2w8m3W7scPYLq2q_veO891mJUQOiZ15nvfejO0HWViFMpEO_wQ0BZ5rKAmq3Ah-aiCvC12D8eKXQ5r_vcQ7L8A131fXTqzIGo9GzZHn12BKG7Hd6AMUN0eRF7YcwTwptSlFhnSrCxSLNnQyX_YjYMC4ZIOe7rvpYoNKuyPoz2Gb1bbR3g_fX31pce07Gj5N0-gsEVnYVtqLlaWuNPE3R1s1SQz8BVLMzWV0fpqnhTB28JVEV5HuOXwAdoIG1gsENc0IB8-hokDnduEgPoLDNhCfbvHpRNybszPx_dsI6Ygm5ppcrMbV1vTJ8OBQrEAfUP2b4i1TfCKyNGNJ06YTzOItDQmfGT-m6SdWtSXgRvbqFm1fs2KPKnCOk3HxpfUo7C2S6PXEzVWSDUaJr3bbbAmN7t2WXqedgqa2zmk23Pk1syK3jGss9mC0q0bCxtLEfidtSHmkCkNTucov2_p_fRsqusGKTYsd2apVMGncGu561_yFWLSX8udb8gqpAoN8PGKRAuGkMnIpqxjWim-LhxjIpd9jhblMeKiAbnKZm0fGQfA2uzeFTDwFXEruQ7mXyQ5Y2FKGWoHv75nhLx_K39aOcz62vF_W7eXU3FGPT6r0g6s)

## * Page View Analytics & Intermittent Data Loss

The blog page views should be recorded in the PostgreSQL database everytime a user views the page. There should be minimal dataloss and the achitecture should provied real time data updates and handle conccurency effectively.

#### * Data Flow

* User lands on the next JS application
* Next JS triggers a request to the django application to log the visit with the necessary page details (url, session)
* Django pushes the event to the redis cache instead of writting to the database first
* A cellery worker, asyncronously processes the page view events from redis, aggregates and writes to the database
* Provide realtime data updates to the frontend through websockets

[
    ![](https://mermaid.ink/img/pako:eNpFkd1uwjAMhV_FyjXsAXoxqT8wIbEJ1gHSUi5M67WBNEFJug1R3n0mk7Y72_qOfXR8FbVtSCSidXju4K2oDEAqN54cbJVXwUOmbQsr68MeptPHMV0tIEetR8jkC32Hh6OHubMmkGn2d3UWsSWLtoq-RshlcUTTWmBlBPIIlME6AmXglRrlRyhkLHh33VHkisitBxoIZp9kAlMzmZMmd4GddSdykZtFLsNQd7Aw7DyMMJcrp3pk8O68dVSul1BkkZ9HfnNuMJCH1KC-BFV7KNB3B4uuGeFJ7uhQ2vpEAXKt-DYrxUT05HpUDed1vW-qROiop0okXDboTpWozI05HIItL6YWSXADTYSzQ9uJ5AO1526IpwuFHHr_Nz2jebf2v-c0OKLn3_fEL91-AEqzkMc?type=png)](https://mermaid.live/edit#pako:eNpFkd1uwjAMhV_FyjXsAXoxqT8wIbEJ1gHSUi5M67WBNEFJug1R3n0mk7Y72_qOfXR8FbVtSCSidXju4K2oDEAqN54cbJVXwUOmbQsr68MeptPHMV0tIEetR8jkC32Hh6OHubMmkGn2d3UWsSWLtoq-RshlcUTTWmBlBPIIlME6AmXglRrlRyhkLHh33VHkisitBxoIZp9kAlMzmZMmd4GddSdykZtFLsNQd7Aw7DyMMJcrp3pk8O68dVSul1BkkZ9HfnNuMJCH1KC-BFV7KNB3B4uuGeFJ7uhQ2vpEAXKt-DYrxUT05HpUDed1vW-qROiop0okXDboTpWozI05HIItL6YWSXADTYSzQ9uJ5AO1526IpwuFHHr_Nz2jebf2v-c0OKLn3_fEL91-AEqzkMc)

#### * Reliability Enhancements

* Caching as a primary write layer - instead of writing to the databse directly, the page views are stored in Redis to avoid write bottlenecks when there are thousands of concurrent requests
* Celery Worker to batch process page views to ensure writes are efficient
* Retry Mechanism - In the event that Redis/Celery fails, use dead letter queues through redis streams to periodically try failed inserts
* High Availability - Have a deployed postgress in multiple zones and also read replicas for scallable analytics queries

[![](https://mermaid.ink/img/pako:eNp1UstuwjAQ_JWVz_TQHjkgkaQ8pFLRglqpCYfF2WKriR35UYQI_17nUQoSvXl3Z-yZ8R4Z1zmxIdsZrASsk0wBWL_tygQdwpKMldaR4tQMAcbp0sgSzQGW2rqdodXLEyTRBu7u4JWqQnJ0UqtQjiBKXwnz3zbcb_orbmHja-xDj406bJi8eDKSbAtO0rHC4uAkt0GmFVuNJu8Z8U3GY_pOWxhXVYsilWfqyuwEfeFgrQsyePY6CZpyaSFGLqhxOKpXThuCNZWVNk0Ib5L2toZpGlOgHpo3PfVKpi1jUngrwOmLvGoY_6dCFvqbDCyIC1TSlt1Ns3RGWDgBsSD-BQutZNDRKUrIEXchh6ile0M1zMMn6VI7gstQe1nzlvZMe-h_soboLIcNWEmmRJmHtTg27Yw5QSVlbBiOOZqvjGXqFHDonV4dFGdDZzwNmNF-J9jwEwsbKl_l6CiRGJyV526F6kPrvzrEG4wsui1sl_H0A2kp2kQ?type=png)](https://mermaid.live/edit#pako:eNp1UstuwjAQ_JWVz_TQHjkgkaQ8pFLRglqpCYfF2WKriR35UYQI_17nUQoSvXl3Z-yZ8R4Z1zmxIdsZrASsk0wBWL_tygQdwpKMldaR4tQMAcbp0sgSzQGW2rqdodXLEyTRBu7u4JWqQnJ0UqtQjiBKXwnz3zbcb_orbmHja-xDj406bJi8eDKSbAtO0rHC4uAkt0GmFVuNJu8Z8U3GY_pOWxhXVYsilWfqyuwEfeFgrQsyePY6CZpyaSFGLqhxOKpXThuCNZWVNk0Ib5L2toZpGlOgHpo3PfVKpi1jUngrwOmLvGoY_6dCFvqbDCyIC1TSlt1Ns3RGWDgBsSD-BQutZNDRKUrIEXchh6ile0M1zMMn6VI7gstQe1nzlvZMe-h_soboLIcNWEmmRJmHtTg27Yw5QSVlbBiOOZqvjGXqFHDonV4dFGdDZzwNmNF-J9jwEwsbKl_l6CiRGJyV526F6kPrvzrEG4wsui1sl_H0A2kp2kQ)

#### * Testing Strategy

* Concurrency & Load Testing - Using locust, simulate 10000+ concurrent page visits and verify redis and celery efficiently handles bulk inserts and ensure PostgreSql can handle high write loads
* DB Unavailability - Similate DB unavailability and ensure failed inserts are retried.
* Data integrity - Compare the redis views and the postgres to ensure there are no duplicates

| Test Case                    | Strategy                  | Expected Outcome                                                |
| ---------------------------- | ------------------------- | --------------------------------------------------------------- |
| 10,000 concurrent page views | load testing using locust | page views are logged with no bottleneck                        |
| Database Unavailability      | Disable Database          | Retry mechanism ensures all the data should be correctly logged |

## * Handling System Failure & Load Spikes

#### * **Objectives**

Ensure high availability under high load and provide a fallback/gracefull degradation if partial systems fail.

#### * Solution

##### * Scaling & Load Balancing

To effectively handle the the traffic spike, we need to implement scalable architecture with load balancing and horizontal scaling

###### * Application Layer Scaling

* Use Gunicorn with multiple workes
* Use uvicorn for realtime features
* For the next js application implement Server Side rendering caching for frequently accessed pages and implement Incremental Static Regeneration to reducle backend workload

###### * Database Scaling

* Use read replicas for scalling read heavy workloads
* optimise queries with indexing on frequently fetched fields

###### * Infrastructure Scaling

* Horizontal scalling - Use multiple instances of the Next JS and Django application behind a load balance
* Implement auto scaling groups to scale down/up servers based on traffic
* Use nginx or AWS ALB to distribute traffic
* implement a CDN through cloudfront to cache static assets

[
    ![](https://mermaid.ink/img/pako:eNplkc1uwyAQhF8FcU6auw-ValuVWrlp_qpItXvYmI1NgsFaQGoU592LHSc5lAPsDPMBYs-8NAJ5xCuCtmabtNAsjC-LxKbT525DsN_LsmNJOs8TZbx4JaPdLMifazRUQzKBskbBkrCL2nVsjr8u76eng2UDhFqMTG8P0MvijSWglO1YFueZAcFiUKBLpDGaxUMwldaR3HmHlvWxjqUH0JXJr0ugyuPj_NHswRWCmG1JOgxInC-MdRXhepmxFBzswOJ_ZunRI3s3u_CsraEjUp6gQjqN6kbE9xumn1qdOrbCVskS8t66iRDmE94gNSBF-OhzDxfc1dhgwaNQCqBjwQt9CTnwzqxPuuSRI48TTsZXNY_2oGxQvhXgMJUQutXc3Rb0tzEPjUI6Qx_Xvg7tvfwBO4ai5A?type=png)](https://mermaid.live/edit#pako:eNplkc1uwyAQhF8FcU6auw-ValuVWrlp_qpItXvYmI1NgsFaQGoU592LHSc5lAPsDPMBYs-8NAJ5xCuCtmabtNAsjC-LxKbT525DsN_LsmNJOs8TZbx4JaPdLMifazRUQzKBskbBkrCL2nVsjr8u76eng2UDhFqMTG8P0MvijSWglO1YFueZAcFiUKBLpDGaxUMwldaR3HmHlvWxjqUH0JXJr0ugyuPj_NHswRWCmG1JOgxInC-MdRXhepmxFBzswOJ_ZunRI3s3u_CsraEjUp6gQjqN6kbE9xumn1qdOrbCVskS8t66iRDmE94gNSBF-OhzDxfc1dhgwaNQCqBjwQt9CTnwzqxPuuSRI48TTsZXNY_2oGxQvhXgMJUQutXc3Rb0tzEPjUI6Qx_Xvg7tvfwBO4ai5A)

##### * Monitoring & Alerting

* Use cloudwatch for cloud native logging
* Use grafana and prometheus to monitor cpu load and memory
* configure alerts on CPU, memory and slow queries that can be delivered through slack for realtime alerting

[
    ](https://mermaid.live/edit#pako:eNpFkEFrwzAMhf-K8bnd7jkMOjLGWDpS0tOcHkSsOmkTKygKrLT971OasBmE_ezvyehdbUUebWIDQ1-bfVpGo2vT9y49QQz0_IU_8nQaDma9frllFIabecs-nZYpBKrzYXZMeiI-4tCEWpR6ZzhCBLfsC7eoB7vnJgRkZTctsrii1X5mAdNXl9MggbHYZfPvuxH5YnLkI3EHsUI15luXgoCnoD67sh3qU-N1ouvUp7RSY4elTfTogc-lLeNdORiFikusbCI84soyjaG2yRHaQdXYexBMG9BYur_bHuI30b9G3wjxdg7wkeP9Fw2JcL0)

[
    ![img](https://mermaid.ink/img/pako:eNpFkEFrwzAMhf-K8bnd7jkMOjLGWDpS0tOcHkSsOmkTKygKrLT971OasBmE_ezvyehdbUUebWIDQ1-bfVpGo2vT9y49QQz0_IU_8nQaDma9frllFIabecs-nZYpBKrzYXZMeiI-4tCEWpR6ZzhCBLfsC7eoB7vnJgRkZTctsrii1X5mAdNXl9MggbHYZfPvuxH5YnLkI3EHsUI15luXgoCnoD67sh3qU-N1ouvUp7RSY4elTfTogc-lLeNdORiFikusbCI84soyjaG2yRHaQdXYexBMG9BYur_bHuI30b9G3wjxdg7wkeP9Fw2JcL0?type=png)](https://mermaid.live/edit#pako:eNpFkEFrwzAMhf-K8bnd7jkMOjLGWDpS0tOcHkSsOmkTKygKrLT971OasBmE_ezvyehdbUUebWIDQ1-bfVpGo2vT9y49QQz0_IU_8nQaDma9frllFIabecs-nZYpBKrzYXZMeiI-4tCEWpR6ZzhCBLfsC7eoB7vnJgRkZTctsrii1X5mAdNXl9MggbHYZfPvuxH5YnLkI3EHsUI15luXgoCnoD67sh3qU-N1ouvUp7RSY4elTfTogc-lLeNdORiFikusbCI84soyjaG2yRHaQdXYexBMG9BYur_bHuI30b9G3wjxdg7wkeP9Fw2JcL0)

##### * Graceful Degradation

* In the event that system failure occurs, instead of showing server errors, downgrade failed feature i.e if the Database fails, defer writes to Redis or a Messaging queue i.e RabbitMq
* Implement a circuit breaker pattern to avoid cascading failures
* oflload all heavy tasks to a background task

## Theoretical System Enhancement

#### * Data Flow

* The next js application site makes a request to the django application, on the server, the Redis cache is checked first to see if there is data, if not the query is made to the DB to fetch the featured top posts
* The following diagram illustrates the process

  [![img](https://mermaid.ink/img/pako:eNp9U01vwjAM_StWTpsE454D0gTTmAQTX7tMvViNaQM0yZJUAyH--9I1rIWK5VA1tt_z80dOLNWCGGeOvkpSKY0lZhaLREE4Bq2XqTSoPHw4sl3rOx3809Z1HeMtqkzD8_yt61uSkA5GmObUdc6185ml1WJa--pvlb0_HMZ0HKYaBUx0QQazSBJ9IarJzeH1ZQ0DNHKwIfSlJdE3IUEU3AQGVEsVh1FO6Q7S6gIbbeGChhYa974Oh4n0tak6LZ7-jZglBRJV015R0d5R5JpJ5xqyK4VNazgsSrLHG1nwsNYGDo8NvEHckdLS8H9DVl5bimlkLCFqV6LTzfaoluSMVgK-pc_b-S7zCrHVdKtAJei21azHCrIFShGW9FQhE-ZzKihhPPwKtLuEJeoc4rD0enVUKePeltRjVpdZzvgGQ3N7rDQC_WW9_6xh5T61bu6h5FDorH4Tv0_j_APwQAd0?type=png)](https://mermaid.live/edit#pako:eNp9U01vwjAM_StWTpsE454D0gTTmAQTX7tMvViNaQM0yZJUAyH--9I1rIWK5VA1tt_z80dOLNWCGGeOvkpSKY0lZhaLREE4Bq2XqTSoPHw4sl3rOx3809Z1HeMtqkzD8_yt61uSkA5GmObUdc6185ml1WJa--pvlb0_HMZ0HKYaBUx0QQazSBJ9IarJzeH1ZQ0DNHKwIfSlJdE3IUEU3AQGVEsVh1FO6Q7S6gIbbeGChhYa974Oh4n0tak6LZ7-jZglBRJV015R0d5R5JpJ5xqyK4VNazgsSrLHG1nwsNYGDo8NvEHckdLS8H9DVl5bimlkLCFqV6LTzfaoluSMVgK-pc_b-S7zCrHVdKtAJei21azHCrIFShGW9FQhE-ZzKihhPPwKtLuEJeoc4rD0enVUKePeltRjVpdZzvgGQ3N7rDQC_WW9_6xh5T61bu6h5FDorH4Tv0_j_APwQAd0)

#### * API & State Management

* The like button makes an api request to update the likes count following the flow illustrated in the diagram below

[![](https://mermaid.ink/img/pako:eNplUk1PwzAM_SuWTyBt9J7DJMQukzYYjF1QLlHidVnbpMSJBJr230nbaQWaQxTb7_n5I2fU3hAKZPpM5DQtrSqDaqSDfFoVotW2VS7CnilMvc_0FR9OPA0sT8qVHh63q2ls6zmWgXav6yE23J3AfLG4ZhTwVFtdMUhc24okDqBrNONGAQHbl907FKq1RZtTc3G25lLUHa0njdDMG8UFrJwO1FAuSWIHZ4lwsFSbgTdC5_8E961RkYCT1sQ8Ufndxhtx6x0T3A0kA70SaJ9cvP_bVeZ1U7jl369whg2FRlmTd3Tu0BLjMdcsUeSnUaHqRnPJOJWi3307jSKGRDMMPpVHFAdVc7ZSn_G63Zs3r-PD-9EmY6MPm-FL9D_j8gOe9LVD?type=png)](https://mermaid.live/edit#pako:eNplUk1PwzAM_SuWTyBt9J7DJMQukzYYjF1QLlHidVnbpMSJBJr230nbaQWaQxTb7_n5I2fU3hAKZPpM5DQtrSqDaqSDfFoVotW2VS7CnilMvc_0FR9OPA0sT8qVHh63q2ls6zmWgXav6yE23J3AfLG4ZhTwVFtdMUhc24okDqBrNONGAQHbl907FKq1RZtTc3G25lLUHa0njdDMG8UFrJwO1FAuSWIHZ4lwsFSbgTdC5_8E961RkYCT1sQ8Ufndxhtx6x0T3A0kA70SaJ9cvP_bVeZ1U7jl369whg2FRlmTd3Tu0BLjMdcsUeSnUaHqRnPJOJWi3307jSKGRDMMPpVHFAdVc7ZSn_G63Zs3r-PD-9EmY6MPm-FL9D_j8gOe9LVD)

##### Like Count Pseudo Code - Backend

```
from django.db.models import F
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt
from django.core.cache import cache
from django.contrib.auth.decorators import login_required
from .models import BlogPost, Like
from rest_framework.throttling import UserRateThrottle
from rest_framework.decorators import throttle_classes

class LikeThrottle(UserRateThrottle):
    rate = '5/min'

@csrf_exempt
@throttle_classes([LikeThrottle])
@login_required
def like_post(request, post_id):
    user = request.user
    if request.method == "POST":
        if Like.objects.filter(user=user, post_id=post_id).exists():
            return JsonResponse({"success": False, "message": "User has already liked this post."}, status=400)
  
        BlogPost.objects.filter(id=post_id).update(likes=F('likes') + 1)
        Like.objects.create(user=user, post_id=post_id)
        return JsonResponse({"success": True, "message": "Like added successfully."})
```

The above code block allowes authenticated users to submit their likes from the frontend  and they are recorded effectively in the database preventing multiple requests by throttling the user requests and ensuring they can only have one like.

##### Likes Code Pseudo Code - Frontend

```
const handleLike = async (postId) => {
    const response = await fetch(`/api/posts/${postId}/like`, { method: 'POST' });
    const data = await response.json();
    if (data.success) {
        setLikes(data.likes);
    } else {
        alert(data.message);
    }
};
```


#### * Hubspot Integration

* Using the HubSpot API key for authentication, when a request has been validated and the like is incremented in the database correctly, a request is made to HubSpot's event tracking system following the pseudo code below

  ```
  import requests

  def track_hubspot_event(user_email, post_id):
      hubspot_endpoint = "https://api.hubapi.com/events/v3/send"
      payload = {
          "email": user_email,
          "eventName": "Post Liked",
          "properties": {"post_id": post_id}
      }
      headers = {"Authorization": "Bearer HUBSPOT_API_KEY"}
      requests.post(hubspot_endpoint, json=payload, headers=headers)
  ```


#### * Security & Validation

* In order to prevent spam likes from web crawlers, we use csrf tokens and a user must be authenticated and have a verified profile to submit a like
* For a user to only like a certain post once, we mantain a seperate model for tracking likes, once a user likes a post we create a record for that like if there is no existing like record

  ```
  if Like.objects.filter(user=user, post_id=post_id).exists():
      return JsonResponse({"success": False, "message": "User has already liked this post."}, status=400)

  ```
* Rate limiting and throttling is implemented to stop the user from submitting too many requests in a short period of time

## Special Instruction Confirmation

For this case study i have followed all the outlined requirements.

* The final submission is a single markdown file
* Each problem is structured as a well thought out theoretical approach which includes scalling approaches, mitigation plans, debugging and disaster recovery
* Each problem is answered in it's own section
* Diagrams are provided where necessary to illustrate the solution
