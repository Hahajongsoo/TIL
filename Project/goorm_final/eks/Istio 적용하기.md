먼저 Istio에서 기본 설정으로 Ingress gateway를 설치하면 LoadBalancer 타입인 service가 생성이된다. 그런데 EKS 상에서는 AWS Load Balancer Controller가 없다면 LoadBalancer타입인 service 생성시 CLB가 만들어지게 된다. 
대신에 ALB, NLB를 만들어주려면 일단 AWS Load Balancer controller를 설치해야 한다. EKS 클러스터 생성시에 이미 이 부분은 마쳤기 때문에 생략한다.

참고

https://aws.amazon.com/ko/blogs/containers/secure-end-to-end-traffic-on-amazon-eks-using-tls-certificate-in-acm-alb-and-istio/

https://itnext.io/deploying-an-istio-gateway-with-tls-in-eks-using-the-aws-load-balancer-controller-448812e081e5