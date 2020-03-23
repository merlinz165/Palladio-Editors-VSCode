# Web-based textual PCM editor for the next-generation IDE Eclipse Che

This repository aims to automatically generate an Eclipse Che workspace template supporting Palladio domain-specific language(DSL).

It consists of a Palladio DSL support extension for VS Code and the corresponding Eclipse devfile generator.

This project is based on the [Palladio Language Server](https://github.com/PalladioSimulator/Palladio-Addons-TextBasedModelGenerator).

![](https://raw.githubusercontent.com/merlinz165/Palladio-Editors-VSCode-Assets/master/images/che-final.png)

## Usage

### Prerequisites
* Java version 8 or higher
* Maven

### Generate VS Code extension and Eclipse Che devfile

1. Specify the necessary variables in `gradle.properties`
Where:
    - `version`: version of the VS Code extension and the corresponding Eclipse Che plug-in (they have the same version).
    - `lsp_download_url`: URL of a runable language server generated by [Palladio-Addons-TextBasedModelGenerator](https://github.com/PalladioSimulator/Palladio-Addons-TextBasedModelGenerator).
    - `vsix_url`: URL where you will place the generated vsix file. Need to be set in advance.
    - `plugin_yaml_url`: URL where you will place the generated `palladio-lsp.yaml` file. Need to be set in advance.

2. Build the project.
3. Put `target/palladio-languageserver.vsix` and `palladio-devfiles/devfile.yaml` on the server, their addresses must be the same as previously set.


### Using Palladio DSL workspace

#### Getting Started with Eclipse Che

Obviously to use the Palladio support plug-in you need a running Eclipse Che. Here you can find links on how to get started with Eclipse Che:

* [Use Eclipse Che online](https://www.eclipse.org/che/getting-started/cloud/)
* [Run Eclipse Che on your own K8S cluster](https://www.eclipse.org/che/docs/che-7/che-quick-starts/)

#### Using Palladio DSL workspace

There are two ways to use the template. You can import the `devfile.yaml` or paste its content when create new workspace in Eclipse Che.
![](https://raw.githubusercontent.com/merlinz165/Palladio-Editors-VSCode-Assets/master/images/create_new_wksp.png)

Or you can create a workspace factory, follow these steps:

1. Log in to Eclipse Che
2. In a web browser, load the following URL:
`https://<CHE_HOST>/f?url=<DEVFILE_URL>`
Where:
    - `https://<CHE_HOST>` specifies the Che Server URL, for example: `https://che.openshift.io`.
    - `/f?url=` factory mechanism links the Che Server URL to the devfile URL.
    - `<DEVFILE_URL>` is the URL where you put your devfile.

3. Press **Enter** and wait for the workspace to initialize.
A workspace is created with Eclipse Palladio plug-in available.

#### Example

Create a new text file with the ending `.trepo` in any project.

``` Smalltalk
Repository textualRepository

import myRepository.*

Types {
    struct MyType  {
        key : STRING
        value : STRING
        type: STRING
    }
    collection MyCollection of STRING
}

Interface IConsumer {
    String invoke (foo: BOOL, bar: BYTE)
    commit
}

Interface IDatabase {
    MyCollection query
}


Component MyConsumer
    provides IConsumer as consumer
    requires IDatabase as database
{
    SEFF on consumer invoke {
        ACQ threadPool
        EXT database -> query
        REL threadPool

  }
  PassiveResource threadPool (10)
}

```
