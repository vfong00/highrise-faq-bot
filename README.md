# highrise-faq-bot

How to use: go to [this link](https://highrise-faq-hxa0qxwrk-vincent-fongs-projects.vercel.app) and simply interact!

How the model was created:
1. Scraped and preprocessed the data, isolating question/answer pairs present in the data.
2. Calculated embeddings of the question/answer pairs. 
3. Calculate most similar pairs to the query semantically based on embeddings.
4. Pass the pulled response(s) into an instruction-tuned LLM to polish the right answer.

The main design choice I made was choosing a threshold to determine what was a relevant answer worth parsing -- I chose 0.5 with decent results. 

Optional features included (and how I would expand upon this further):
1. Basic NLP techniques: semantics are considered already, but I would probably do some kind of keyword extraction on top of what I already have.
2. User feedback: currently the user can rate generated responses, we can probably improve on the log storage.

Sample tracking logs are below:
```js
2024-11-28 02:44:21,920 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:21] "GET / HTTP/1.1" 200 -
2024-11-28 02:44:22,673 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:22] "GET /static/styles.css HTTP/1.1" 304 -
2024-11-28 02:44:28,249 - INFO - USER QUERY ERROR - model's similarity score too low to be deemed useful
2024-11-28 02:44:28,249 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:28] "POST /search HTTP/1.1" 200 -
2024-11-28 02:44:28,274 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:28] "GET /couldnot HTTP/1.1" 200 -
2024-11-28 02:44:28,372 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:28] "GET /static/styles.css HTTP/1.1" 304 -
2024-11-28 02:44:30,517 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:30] "GET / HTTP/1.1" 200 -
2024-11-28 02:44:30,682 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:30] "GET /static/styles.css HTTP/1.1" 304 -
2024-11-28 02:44:39,968 - INFO - USER QUERY - Query: how does the delete option work?, Response: To delete an item, go to the My Items section, tap and hold the item, select "Delete Item," confirm the action, and receive Bubbles as a refund.
2024-11-28 02:44:39,969 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:39] "POST /search HTTP/1.1" 200 -
2024-11-28 02:44:40,005 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:40] "GET /response?query=how%20does%20the%20delete%20option%20work?&answer=To%20delete%20an%20item,%20go%20to%20the%20My%20Items%20section,%20tap%20and%20hold%20the%20item,%20select%20"Delete%20Item,"%20confirm%20the%20action,%20and%20receive%20Bubbles%20as%20a%20refund.&source_url=https://support.highrise.game/en/articles/8896141-deleting-items HTTP/1.1" 200 -
2024-11-28 02:44:40,074 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:40] "GET /static/styles.css HTTP/1.1" 304 -
2024-11-28 02:44:42,350 - INFO - FEEDBACK - Query: how does the delete option work?, Answer: To delete an item, go to the My Items section, tap and hold the item, select &#34;Delete Item,&#34; confirm the action, and receive Bubbles as a refund., Feedback: good
2024-11-28 02:44:42,351 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:42] "POST /feedback HTTP/1.1" 200 -
2024-11-28 02:44:45,899 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:45] "GET / HTTP/1.1" 200 -
2024-11-28 02:44:45,961 - INFO - 127.0.0.1 - - [28/Nov/2024 02:44:45] "GET /static/styles.css HTTP/1.1" 304 -
```