# Task Below

create a pod named pod1 that contains an nginx docker container image .  

create a pod named pod 2 that contains an httpd docker container image and a label named os=alpine

display the content of the rs.yaml replicaset definition file.

create a pod replicaset by using rs.yaml file. display the content of the webservers.yaml deployment definition file.

create a deployment by using the webservers.yaml file.

create a deployment file named database.yaml for a deployment named database that contains a redis docker container image.

create a deployment by using the database.yaml file . determine the number of pods that are running in the default namespace

## Solution Below

1. Create a pod named pod1 that contains an nginx Docker container image: After defining the manifest file pod1.yaml, Create the pod using the following command: `kubectl apply -f pod1.yaml`

2. Create a pod named pod2 that contains an httpd Docker container image and a label named os=alpine: After defining the manifest file pod2.yaml, Create the pod using the following command: `kubectl apply -f pod2.yaml`

3. Display the content of the rs.yaml ReplicaSet definition file: define the manifest file rs.yaml:

4. Create a pod ReplicaSet using the rs.yaml file: `kubectl apply -f rs.yaml`

5. Display the content of the webservers.yaml Deployment definition file:  define the manifest file webservers.yaml,

6. Create a Deployment using the webservers.yaml file: `kubectl apply -f webservers.yaml`

7. Create a Deployment file named database.yaml for a Deployment named database that contains a redis Docker container image:Define the database.yaml

8. Create a Deployment using the database.yaml file: `kubectl apply -f database.yaml`

9. Determine the number of pods that are running in the default namespace: `kubectl get pods -n default | grep -c 'Running'` . This command will return the number of pods with the status 'Running' in the default namespace.

10. To determine the number of ReplicaSets running in the default namespace, you can use the `kubectl` command-line tool. Here's how: `kubectl get replicaset -n default`

This command will display a table of information, including the ReplicaSet names, the number of desired and current replicas, and more.

**11.**To get the total number of ReplicaSets in the default namespace, you can use the following command: `kubectl get replicaset -n default | awk 'NR>1 {count++} END {print count}'`

*12.*This command above retrieves all ReplicaSets in the default namespace and uses the awk tool to count the number of lines (excluding the header line) and print the count.

**13.**create a namespace called finance create a pod named pod 3 in the finance namespace that contains a  redis docker container image and a label named app = accounting. First, let's create a namespace called "finance" using: `kubectl create namespace finance`

*14.*Now, create a pod named pod3 in the finance namespace that contains a redis Docker container image and a label named app=accounting: lookup the pod3.yaml file.

$$
15.
$$create the pod using this command: `kubectl apply -f pod3.yaml`

This command above  will create a pod named "pod3" with a redis container and the label app=accounting in the finance namespace.

*** Take Note that the `awk` command and `sed` commands are two powerful used text manipulation tools in `Linux`

awk and sed are two powerful and widely used text manipulation tools in Linux.

awk stands for "Aho, Weinberger, and Kernighan," named after the three computer scientists who created it. It is a command-line utility used for processing and manipulating text files in Linux. awk reads input files line by line, and for each line, it performs a set of actions specified by the user. It can be used to extract specific columns of data from files, perform calculations, and transform data into a different format. awk is particularly useful for working with structured data files, such as CSV files or log files.

Here's an example of how to use awk to extract the second column of a CSV file below

```markdown
awk -F "," '{print $2}' myfile.csv

```

In this example, the -F option specifies the delimiter (comma in this case), and '{print $2}' tells awk to print the second column of each line.

sed stands for "Stream Editor" and is another text manipulation tool commonly used in Linux. sed can be used to perform various text transformations on input data, such as substituting text, deleting lines, and inserting or appending text to a file. Like awk, sed reads input files line by line and applies a set of rules or commands to each line.

Here's an example of how to use sed to replace all occurrences of the word "hello" with "hi" in a file:

```markdown
sed 's/hello/hi/g' myfile.txt
```

In this example, the s command substitutes "hello" with "hi" (s/hello/hi/), and the g option tells sed to replace all occurrences on each line.

Both awk and sed are powerful tools for manipulating text data in Linux, and mastering them can save you a lot of time and effort when working with text files.
