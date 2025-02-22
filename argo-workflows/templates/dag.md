A DAG template is another common type of template, lets look at a complete example:

```
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: dag-
spec:
  entrypoint: main
  templates:
    - name: main
      dag:
        tasks:
          - name: a
            template: whalesay
          - name: b
            template: whalesay
            dependencies:
              - a
    - name: whalesay
      container:
        image: docker/whalesay
        command: [ cowsay ]
        args: [ "hello world" ]

```

In this example, we have two templates:

* The "main" template is our new DAG.
* The "whalesay" template is the same template as in the container example.

The DAG has two tasks: "a" and "b". Both run the "whalesay" template, but as "b" depends on "a", it won't start until "
a" has completed successfully.

Let's run the workflow:

`argo submit --watch dag-workflow.yaml`{{execute}}

Open the Argo Server tab a navigate to the workflow, you should see a two containers.

## Exercise

Add a new task named "c" to the DAG. Make it depend on both "a" and "b". See how this looks in the user interface.