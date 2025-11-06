interface - objects shape
interface User = {
name: string;
}
interface Admin extends User {}

types - unions or primitives
type User = {name: string;} | {name: number;}
type Admin = User & {role: string;}

tuples - fixed length array with types
const user: [string, number] = ["John", 20];

utility- functions to manipulate data types
Required<T>
const user: Required<User> = { name: "John", age: 20 };
Readonly<T> - make all properties readonly

Generic - placeholder for a type
function identity<T>(arg: T): T { return arg; }
identity<string>("hello");

FireStore DB
db.collection("users").doc("u1").
where("age", ">", 20).get();
where("role", "in", ["admin", "manager"])
add
set
get
delete()
({
name: "Kalidas",
age: 27,
active: true
});

// cloud services
cloudrun - serverless compute
( 60 mins
When need full container flexibility but still want serverless scaling, particularly for HTTP APIs, microservices, workers, or batch jobs.
)
EC2 - compute engine
lambda - cloud functions
s3 - cloud storage
cloudwatch - monitoring
cloudtrail - audit logs
route 53 - dns
vpc - virtual private cloud
iam - identity and access management
sns - simple notification service
sqs - simple queue service
stepfunctions - workflow orchestration

// deep copy vs shallow copy

const shallow = { ...original };
const deep = structuredClone(original);
