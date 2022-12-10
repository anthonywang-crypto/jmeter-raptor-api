## 用例转换关系规划
<img width="775" alt="image" src="https://user-images.githubusercontent.com/104351659/206644514-990a3939-cbf1-489f-95ae-58347958cfd3.png">


## V4.3版本实现逻辑 
<img width="783" alt="image" src="https://user-images.githubusercontent.com/104351659/206644974-d6472fd7-d77c-4f98-949d-08cce3a4681d.png">


## httprunner 以pytest的模式编写用例的模版

```python
import os
import sys

sys.path.insert(0, os.getcwd())

from httprunner import HttpRunner, Config, Step, RunRequest, RunTestCase

from examples.postman_echo.request_methods.request_with_functions_test import (
    TestCaseRequestWithFunctions as RequestWithFunctions,
)


class TestCaseRequestWithTestcaseReference(HttpRunner):
    config = (
        Config("request methods testcase: reference testcase")
        .variables(
            **{
                "foo1": "testsuite_config_bar1",
                "expect_foo1": "testsuite_config_bar1",
                "expect_foo2": "config_bar2",
            }
        )
        .base_url("https://postman-echo.com")
        .verify(False)
    )

    teststeps = [
        Step(
            RunTestCase("request with functions")
            .with_variables(
                **{"foo1": "testcase_ref_bar1", "expect_foo1": "testcase_ref_bar1"}
            )
            .call(RequestWithFunctions)
            .export(*["foo3"])
        ),
        Step(
            RunRequest("post form data")
            .with_variables(**{"foo1": "bar1"})
            .post("/post")
            .with_headers(
                **{
                    "User-Agent": "HttpRunner/${get_httprunner_version()}",
                    "Content-Type": "application/x-www-form-urlencoded",
                }
            )
            .with_data("foo1=$foo1&foo2=$foo3")
            .validate()
            .assert_equal("status_code", 200)
            .assert_equal("body.form.foo1", "bar1")
            .assert_equal("body.form.foo2", "bar21")
        ),
    ]


if __name__ == "__main__":
    TestCaseRequestWithTestcaseReference().test_start()

```

  ### 核心语法规则
  
  主要是几个标红的关键字
  <img width="225" alt="image" src="https://user-images.githubusercontent.com/104351659/206698536-4b99af70-7358-47bb-8935-dfbe8d3b6a7a.png">
  
  说明：
  - config
    base_url (optional): testcase的统一请求host前缀
    export (optional)： testcase 提取的对外变量
  
  - teststeps
    - RunRequest(name)
      .method(url)： http api请求的方法(get/post/put/delete...)
      .with_data: form_date格式请求的数据
      .with_json： json格式请求的参数数据 
      
    - RunTestCase(name)
      .call： **链式调用**的关键，指定依赖的test case
      .export： 从依赖的case中提取的session variable
    
  
  ## httprunner与pytest结合的探索
  
  ### 1. 查看API用例的更多详情（request data/respond data/extraction and validation）
  argument -s (shortcut for --capture=no)
  <img width="1419" alt="image" src="https://user-images.githubusercontent.com/104351659/206701825-b7823385-cf5d-4f49-ac2e-d8a7f9f990d2.png">
  
  ### 2. 结合pytest的用例标记（#todo）
  
  ### 3. allure报告的生成
  - 直接使用pytest的原生命令，如下：
  `pytest -s testcases/pytest_demo/requests_demo_test.py  --alluredir=allure_result `
  
  - 如果使用hrp命令会报错，不识别的 --alluredir 参数：
  <img width="1224" alt="image" src="https://user-images.githubusercontent.com/104351659/206866023-02113bb4-cb67-4a17-96d4-ad05d4d4a5a8.png">

  - allure报告生成命令
  ```
    # 在线打开报告
    $ allure serve allure_result
    # 生成html报告
    allure generate allure_result -o reports/allure/html
 ```
  

  
