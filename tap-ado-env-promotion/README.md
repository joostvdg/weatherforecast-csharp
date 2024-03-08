# Automated GitOps Environment Promotion for DotNet app in ADO via TAP

## Diagram


```sh
           +-----------+     +-----------+
           |  Checkout | --> |   Build   |
           +-----------+     +-----------+
                 |                |
                 v                v
            +------+         +-----------+
            | Test |         |   Scan    |
            +------+         +-----------+
                               |
                               v
    +----------------------+   +-----------------------+
    | PR to Git Repository |   | PR to Git Repository |
    |    (env-test)        |   |     (env-pre)        |
    +----------------------+   +-----------------------+
                 |                          |
                 v                          v
      +------------------+    +------------------+
      |   Namespace      |    |    Namespace     |
      |   (App-Test)     |    |     (App-Pre)    |
      +------------------+    +------------------+
          |                      |
          v                      v
      +------------+          +------------+
      | Deliverable|          | Deliverable|
      +------------+          +------------+
      +--------------+     +--------------+
      | Configuration|     | Configuration|
      |  Slice       |     |  Slice       |
      +--------------+     +--------------+
      +--------------+     +--------------+
      | ConfigMap    |     | ConfigMap    |
      +--------------+     +--------------+

```


```mermaid
graph TD;
    Checkout --> Build;
    Build --> Test;
    Test --> Scan;
    Scan --> |"PR to Git Repository (env-test)"| PR_Test;
    Scan --> |"PR to Git Repository (env-pre)"| PR_Pre;
    PR_Test --> Namespace_AppTest;
    PR_Pre --> Namespace_AppPre;
    Namespace_AppTest --> Deliverable_Test;
    Namespace_AppPre --> Deliverable_Pre;
    Deliverable_Test --> ConfigurationSlice_Test;
    Deliverable_Test --> ConfigMap_Test;
    Deliverable_Pre --> ConfigurationSlice_Pre;
    Deliverable_Pre --> ConfigMap_Pre;

    subgraph Namespace
        Namespace_AppTest
        Namespace_AppPre
    end

    subgraph Deliverable
        Deliverable_Test
        Deliverable_Pre
    end

    subgraph Configuration
        ConfigurationSlice_Test
        ConfigMap_Test
        ConfigurationSlice_Pre
        ConfigMap_Pre
    end
```