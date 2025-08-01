```bash
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: get-cyberark-secret
spec:
  params:
    - name: secret-name
      description: The CyberArk credential name
  results:
    - name: sql-username
    - name: sql-password
  steps:
    - name: fetch-secret
      image: company/cyberark-client:latest
      script: |
        #!/bin/sh
        response=$(curl -s -X GET \
          -H "Authorization: Token token=<CYBERARK_AUTH_TOKEN>" \
          "https://cyberark.url.com/secrets/v1/secret?name=$(params.secret-name)")

        echo -n "$(echo $response | jq -r .username)" > $(results.sql-username.path)
        echo -n "$(echo $response | jq -r .password)" > $(results.sql-password.path)
```
