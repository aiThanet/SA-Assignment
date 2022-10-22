# SA Assignment : AWS Solution Proposal
Author: Thanet Sirichanyaphong

# Table of Contents
- [SA Assignment : AWS Solution Proposal](#sa-assignment--aws-solution-proposal)
- [Table of Contents](#table-of-contents)
- [Context](#context)
- [Solution](#solution)
  - [A. Troubleshooting](#a-troubleshooting)
  - [B. Short term solution](#b-short-term-solution)
  - [C. Long term solution](#c-long-term-solution)
- [References](#references)


# Context
Our customers have no concrete AWS knowledge background and are encountering an issue when launching a web application as a proof of concept. Therefore they contact us seeking guidance. First, troubleshooting the current implementation to make the web application operational with minimum effort. Secondly, proposing a short term solution to improve the availability, security, reliability, cost and performance before the project goes live. Lastly, they want to have a long term solution plan to operate their website on a large scale.


# Solution

## A. Troubleshooting
``` diff
diff --git a/SA-Assignment-Thanet-Sirichanyaphong.yaml.yaml b/SA-Assignment-Thanet-Sirichanyaphong.yaml.yaml
index 6eb008e..3d8bd92 100644
--- a/SA-Assignment-Thanet-Sirichanyaphong.yaml.yaml
+++ b/SA-Assignment-Thanet-Sirichanyaphong.yaml.yaml
@@ -148,12 +148,34 @@ Resources:
           DeviceIndex: 0
           SubnetId: !Ref 'PublicSubnetA'
           GroupSet: [!Ref 'SASGapp']
+
+    SAInstance2:
+      Type: AWS::EC2::Instance
+      Properties:
+        DisableApiTermination: 'false'
+        InstanceInitiatedShutdownBehavior: stop
+        ImageId: ami-047bb4163c506cd98
+        InstanceType: t2.micro
+        Monitoring: 'false'
+        UserData: IyEvYmluL2Jhc2gNCnl1bSB1cGRhdGUgLXkNCnl1bSBpbnN0YWxsIC15IGh0dHBkMjQNCnNlcnZpY2UgaHR0cGQgc3RhcnQNCmNoa2NvbmZpZyBodHRwZCBvbg0KZ3JvdXBhZGQgd3d3DQp1c2VybW9kIC1hIC1HIHd3dyBlYzItdXNlcg0KY2hvd24gLVIgcm9vdDp3d3cgL3Zhci93d3cNCmNobW9kIDI3NzUgL3Zhci93d3cNCmZpbmQgL3Zhci93d3cgLXR5cGUgZCAtZXhlYyBjaG1vZCAyNzc1IHt9ICsNCmZpbmQgL3Zhci93d3cgLXR5cGUgZiAtZXhlYyBjaG1vZCAwNjY0IHt9ICsNCmVjaG8gJzxodG1sPjxoZWFkPjx0aXRsZT5TdWNjZXNzITwvdGl0bGU+PC9oZWFkPjxib2R5PjxpZnJhbWUgd2lkdGg9IjU2MCIgaGVpZ2h0PSIzMTUiIHNyYz0iaHR0cHM6Ly93d3cueW91dHViZS5jb20vZW1iZWQvSnMyMXhLTUZkd3ciIGZyYW1lYm9yZGVyPSIwIiBhbGxvd2Z1bGxzY3JlZW4+PC9pZnJhbWU+PC9ib2R5PjwvaHRtbD4nID4gL3Zhci93d3cvaHRtbC9kZW1vLmh0bWw=
+        Tags:
+          - Key: environment
+            Value: sa-assignment
+          - Key: Name
+            Value: !Join ['-', [Instance2, !Ref 'CandidateName']]
+        NetworkInterfaces:
+        - AssociatePublicIpAddress: 'true'
+          DeleteOnTermination: 'true'
+          Description: Primary network interface
+          DeviceIndex: 0
+          SubnetId: !Ref 'PublicSubnetB'
+          GroupSet: [!Ref 'SASGapp']
           
     SAelb:
       Type: AWS::ElasticLoadBalancing::LoadBalancer
       Properties:
-        Subnets: [!Ref 'PublicSubnetB']
-        Instances: [!Ref 'SAInstance1']
+        Subnets: [!Ref 'PublicSubnetA', !Ref 'PublicSubnetB']
+        Instances: [!Ref 'SAInstance1', !Ref 'SAInstance2']
         SecurityGroups: [!Ref 'SASGELB']
         Listeners:
         - LoadBalancerPort: '80'
@@ -162,7 +184,7 @@ Resources:
         HealthCheck:
           HealthyThreshold: '2'
           Interval: '15'
-          Target: TCP:443
+          Target: HTTP:80/demo.html
           Timeout: '5'
           UnhealthyThreshold: '2'
         Tags:
@@ -176,6 +198,12 @@ Resources:
       Properties:
         GroupDescription: SA Assignment - ELB security group
         VpcId: !Ref 'SAVPC'
+        SecurityGroupIngress:
+          - IpProtocol: tcp
+            FromPort: 80
+            ToPort: 80
+            CidrIp: 0.0.0.0/0
+            Description: Accessible to http
         Tags:
           - Key: environment
             Value: sa-assignment
@@ -187,6 +215,12 @@ Resources:
       Properties:
         GroupDescription: SA Assignment - App server security group
         VpcId: !Ref 'SAVPC'
+        SecurityGroupIngress:
+          - IpProtocol: tcp
+            FromPort: 80
+            ToPort: 80
+            CidrIp: 0.0.0.0/0
+            Description: Accessible to http
         Tags:
           - Key: environment
             Value: sa-assignment
```

## B. Short term solution
## C. Long term solution

# References

