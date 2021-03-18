# gographer
Simple graph package for go. Uses [d3js](https://github.com/mbostock/d3) for visualization and websockets for communication. 

IMPORTANT -- This repo is under active developement and should not be used in production until we acheive v1.0.0 
Our initial release is 0.1.0 which is unstable at best. 
https://semver.org/


This package is a fork from "github.com/fjukstad/gographer"

We Fixed various websocket issues. We using the standard lib websockets. 
We have consolidated all relavant methods into on file.
Once it hits v 1.0 consider it stable and production ready. Our goal is to use this repo to build out a richer set of visualazions based on d3js and then produce a scalable graph db using go. 

# Run the test visualization

    go run test_graph/visualization.go
    
and visit [localhost:8080](http://localhost:8080) in your browser 

# Using it:
## Step 1) create a goland project.
## Step 2) copy the below file into the root and call it main.go

```golang:
package main

import (
	"fmt"
	"github.com/raymondbernard/go-grapher/gographer"
	"net/http"
	"os"
)

func main() {
	// make sure you set you export your gopath!
	gopath := os.Getenv("GOPATH")

	rootServeDir := gopath + "/pkg/mod/github.com/raymondbernard/go-grapher@v0.1.0/root_serve_dir/"
	// make sure the rootServerDir exsists!
	// If not, at your command pront in your root  > go get github.com/raymondbernard/go-grapher
	fmt.Println(rootServeDir)

	// init graph
	graph := gographer.NewG()

	// Create graph nodes
	// (ID, NodeText, GroupID, Size)
	graph.AddNode(1, "Node Text blah", 1234, 1)
	graph.AddNode(2, "Node Text blah 2", 1234, 1)

	// Create graph relationships with edges
	// (Source, Target, EdgeID, weight)
	graph.AddEdge(1, 2, 0, 1)
	graph.AddEdge(2, 1, 100, 15)

	// graph.json represent the graph you created
	graph.DumpJSON("graph.json")
	// server up graph on port 8080
	http.ListenAndServe(":8080", http.FileServer(http.Dir(".")))
}

```

## Run the two command in your project.

```cmd:

    go mod init 

    go mod tidy
    
```


## 5) Open browser: 

    Open [localhost:8080](http://localhost:8080) in a webbrowser to view the graph.


    Modify the d3js implementation and visualization to your preferences.


## Files: 
    All files for our pkgs are kept below
    gopath + "/pkg/mod/github.com/raymondbernard/go-grapher@v0.1.0/"


### Sample vizualization 

- test_graph/visualizer.go contains source code to host the visualizations.  They are polulated automatically.
- test_graph/graphtest.go  contains source code to host the visualizations. This is the sample file which creates two nodes.
- test_graph/fixed_layout.go  contains source code to host the visualizations. This sample file crates a fixed layout.  I
The d3 graph stops moving after a sec or two. 


### Core Lib

- gographer/gographer.go  contains source code to get started with our graph library

### Web files 
- root_serve_dir/index.html contains the empty shell for the visualization
- root_serve_dir/js/ contains source code for the visualization
- root_serve_dir/css/ contains css files for visualization

### Json
This file is autogenerated when you run your program
- graph.json contains the output from gographer.go (graph).
