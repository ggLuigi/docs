---
id: mqg2951r8ndcyy340hj5y07
title: Kibana
desc: ''
updated: 1646227390030
created: 1646226237919
---

## Installation
Similar to [[Elasticsearch installation|dev.elasticsearch#installation]]
Only need to run the command to bring up the Kibana
```sh
bin\kibana.bat #or
bin/kibana
```
It requires the Elasticsearch to be running first.

For security enabled login
1. Then open in the browser with [http://localhost:5601](http://localhost:5601)
   Or click the link shown on the console log
2. Paste the enrollment token that you copied and click the button to connect your Kibana instance with Elasticsearch. </br>
   If the token expires (only last for 30mins), generate new token with command:
   ```sh
   bin\elasticsearch-create-enrollment-token.bat -s kibana
   ```
3. Log in to Kibana as the elastic user with the password that was generated when you started Elasticsearch</br>
   ```
   user: elastic
   pw: xxxxxxxxxxxx
   ```

## Config
Change any configurations in `kibana.yml` inside `config/` folder.
e.g. Ip address, port


