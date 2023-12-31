Problem Statement 2:
Assume, you have a BI dashboard application/tool which is deployed on EKS. The Nginx ingress controller is used for exposing the application outside the cluster. There are many webpages of Dashboard application which can be exposed outside -
a. Overview Dashboard page which is accessible at /dashboard/app1/overview
b. Login page is accessible at /login/
c. All charts are visible at /charts/list
d. All dashboards are accessible at /dashboard/list
e. User management page is accessible at /users/list
f. Role management page is accessible at /roles/list
Note:
1. Assume DNS of the application is https://patho.roche.com
2. When user hits the DNS of the application, he should see Overview Dashboard page
(mentioned above in point #a) but the URL should not show /dashboard/app1/overview.
3. None of the page from point #b to point #f should be accessible. Error 503 should be shown if the user provides these URLs. Ex: If user hits https://patho.roche.com/login/
then Error 503 should be shown because login page should not be shown as mentioned in point #2
Provide/update the below nginx ingress manifest mentioned below to handle this -
apiVersion: networking.k8s.io/v1 kind: Ingress
metadata:
annotations: nginx.ingress.kubernetes.io/session-cookie-expires: "86500" nginx.ingress.kubernetes.io/session-cookie-max-age: "86500" nginx.ingress.kubernetes.io/session-cookie-name: app1_cookie
creationTimestamp: "2023-10-03T09:22:54Z" generation: 2
labels:
app: app1
chart: app1-0.10.2 heritage: Helm release: app1
name: app1 namespace: app1-ns
  
resourceVersion: "2100231"
uid: abc111-1110-1111 spec:
ingressClassName: nginx rules:
- host: patho.roche.com
http: paths:
- backend:
service: name: app1 port:
number: 80 path: /
pathType: ImplementationSpeci c status:
loadBalancer:
ingress:
- hostname: axxxxxxxxxx-1000000.eu-west-1.elb.amazonaws.com
