# Find matching tags ghapi

This action allows using regex and github API to filter a repository's tags.

## Inputs

| name |  description |  required |  default |
|---|---|---|---|
| regex |  regex to match a tag with |  true |  |
| owner |  repository owner username or organization name |  true |  |
| repo |  repository name |  true |  |
| token |  github token used to authenticate API user. see https://docs.github.com/en/rest/overview/resources-in-the-rest-api#requests-from-github-actions |  true |  |
| per_page |  amount of results per page to fetch. Between [1-100] |  false |  30 |
| page |  page number to fetch from |  false |  1 |
| sort |  sorting of output tags. asc or desc |  false |  asc |

## Outputs

| name |  description |
|---|---|
| tags |  JSON array of matched tags |

## Example usage

```yml
- name: Find matching tags
  id: find_matching_tags
  uses: Rochet2/find-matching-tags-ghapi@v1.1
  with:
    regex: ^TEST.*\.
    sort: asc
    owner: Rochet2
    repo: find-matching-tags-ghapi
    token: ${{ secrets.GITHUB_TOKEN }}

- name: Assert tags
  uses: nick-fields/assert-action@ed2cc40e8584b4abbce95fc9dd291391c685443f
  with:
    expected: '["TEST-1","TEST-2","TEST-3"]'
    actual: ${{ steps.find_matching_tags.outputs.tags }}

- name: Print outputs
  run: echo "Output ${{ steps.find_matching_tags.outputs.tags }}"

- name: Print first tag
  run: echo ${{ fromJson(steps.find_matching_tags.outputs.tags)[0] }}
```
