```yml
- name: Search manual alerts
  uri:
    url: "https://example.com"
    method: GET
    headers:
      Authorization: "Bearer {{ bearer_token }}"
    return_content: true
    status_code: 200
  register: api_response
  retries: 1
  delay: 3
```