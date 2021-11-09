```python
import requests
import json
import time 
name = "repositories/gitlab/gitlab-ce"

def query_gitlabce_tags():
  all_tags = []
  query_tag_url = "https://hub.docker.com/v2/repositories/gitlab/gitlab-ce/tags/?page_size=100&page=1"
  while query_tag_url:
    resp = requests.get(query_tag_url)
    resp_json = json.loads(resp.text)
    tag_results = [i["name"] for i in resp_json["results"]]
    all_tags += tag_results
    query_tag_url = resp_json["next"]
    time.sleep(2)
  
  print(len(all_tags))
  return all_tags
```

经检测发现，小版本号内的css文件名基本一致，也就是说，基于css文件只能确定gitlab的大版本号,可能极个别情况下会出现小版本不一致情况，从而精确到具体小版本号，例如 `14.2.6-ce.0` ; 其中rc 版本又和ce版本不同，例如 `13.7.0-rc3.ce.0` 。

官网tags大概有[934](https://hub.docker.com/v2/repositories/gitlab/gitlab-ce/tags/?page_size=100&page=1)个，为了节省时间，只收集 `x.x.0-ce.0 ` 版本，大概71个。

```cmd
14.4.0-ce.0
14.3.0-ce.0
14.2.0-ce.0
14.1.0-ce.0
14.0.0-ce.0
13.12.0-ce.0
13.11.0-ce.0
13.10.0-ce.0
13.9.0-ce.0
13.8.0-ce.0
13.7.0-ce.0
13.6.0-ce.0
13.5.0-ce.0
13.4.0-ce.0
13.2.0-ce.0
13.1.0-ce.0
13.0.0-ce.0
12.10.0-ce.0
12.9.0-ce.0
12.8.0-ce.0
12.7.0-ce.0
12.6.0-ce.0
12.5.0-ce.0
12.4.0-ce.0
12.3.0-ce.0
12.2.0-ce.0
12.1.0-ce.0
12.0.0-ce.0
11.11.0-ce.0
11.10.0-ce.0
11.9.0-ce.0
11.8.0-ce.0
11.7.0-ce.0
11.6.0-ce.0
11.5.0-ce.0
11.4.0-ce.0
11.3.0-ce.0
11.2.0-ce.0
11.1.0-ce.0
11.0.0-ce.0
10.8.0-ce.0
10.7.0-ce.0
10.6.0-ce.0
10.5.0-ce.0
10.4.0-ce.0
10.3.0-ce.0
10.2.0-ce.0
10.1.0-ce.0
10.0.0-ce.0
9.5.0-ce.0
9.4.0-ce.0
9.3.0-ce.0
9.2.0-ce.0
9.1.0-ce.0
9.0.0-ce.0
8.17.0-ce.0
8.16.0-ce.0
8.15.0-ce.0
8.14.0-ce.0
8.13.0-ce.0
8.12.0-ce.0
8.11.0-ce.0
8.9.0-ce.0
8.8.0-ce.0
8.7.0-ce.0
8.6.0-ce.0
8.5.0-ce.0
8.4.0-ce.0
8.3.0-ce.0
8.2.0-ce.0
8.0.0-ce.0
```

直接提取[jas502n/GitlabVer](https://github.com/jas502n/GitlabVer) 里面的文件hash以及匹配[gitlab_versions_map.txt](https://github.com/righel/gitlab-version-nse/blob/main/gitlab_versions_map.txt)得到常见的272个。



## 参考链接：

通过提取 `/assets/webpack/manifest.json` 的hash字段获取版本。

https://github.com/righel/gitlab-version-nse/blob/main/gitlab_versions_map.txt

https://censys.io/blog/cve-2021-22205-it-was-a-gitlab-smash/

https://hub.docker.com/r/gitlab/gitlab-ce/tags

https://hub.docker.com/v2/repositories/gitlab/gitlab-ce/tags/?page_size=100&page=1

https://github.com/jas502n/GitlabVer

