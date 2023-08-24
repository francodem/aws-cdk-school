# AWS Cloud Development Kit (CDK)

AWS CDK is a framework to create IaaC through programming languages such as Python, Typescript, Javascript, and more.

## Main Functionalities
- Build with high-level constructs that automatically provide sensible, secure defaults for your AWS resources, defining more infrastructure with less code.
- Use programming idioms like parameters, conditionals, loops, composition, and inheritance to model your system design from building blocks provided by AWS and others.
- Put your infrastructure, application code, and configuration all in one place, ensuring that every milestone you have a complete, cloud-deployable system.
- Employ software engineering practices such as code reviews, unit tests, and source control to make your infrastructure more robust.
- Connect your AWS resources together (even across stacks) and grant permissions using simple, intent-oriented APIs.
- Import existing AWS CloudFormation templates to give your resources a CDK API.
- Use the power of AWS CloudFormation to perform infrastructure deployments predictably and repeatedly, with rollback on error.
- Easily share infrastructure design patterns among teams within your organization or event with the public.

Developers can use any of the supported programming languages to define **reusable** cloud components known as **Constructs**. You compose these together into [Stacks](https://docs.aws.amazon.com/cdk/v2/guide/stacks.html) and [Apps](https://docs.aws.amazon.com/cdk/v2/guide/apps.html).

![Alt Figure: Functional digram.](https://docs.aws.amazon.com/images/cdk/v2/guide/images/AppStacks.png)
**Figure.** How CDK works and its components.

## Installation

First install CDK and Constructs library in a virtual environment previously created in the machine.

```bash
pip install aws-cdk-lib constructs
```

## Usage

```python
from aws_cdk import (
    aws_lambda as _lambda,
    App,
    RemovalPolicy,
    Stack
)


class LambdaLayerStack(Stack):
    def __init__(self, app: App, id: str) -> None:
        super().__init__(app, id)

        # create layer
        layer = _lambda.LayerVersion(
            self,
            'helper_layer',
            code=_lambda.Code.from_asset('layer'),
            description='Common helper utility',
            compatible_runtimes=[
                _lambda.Runtime.PYTHON_3_6,
                _lambda.Runtime.PYTHON_3_7,
                _lambda.Runtime.PYTHON_3_8,
            ],
            removal_policy=RemovalPolicy.DESTROY
        )

        # create lambda function
        function = _lambda.Function(
            self,
            'lambda_function',
            runtime=_lambda.Runtime.PYTHON_3_8,
            handler='index.handler',
            code=_lambda.Code.from_asset('lambda'),
            layers=[layer]
        )
    
app = App()
LambdaLayerStack(
    app,
    'LambdaLayerExample'
)
app.synth()
```