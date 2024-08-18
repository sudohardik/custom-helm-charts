Steps:

1. Create chart and delete unnecessary files and configure application image in values.yaml
2. Update Deployment
3. Configure NodePort in values.yaml
4. Add MySql Dependency in chart.yaml
5. Pass Values like root password, db etc in values.yaml
6. Use ConfigMap
7. Release and Test
8. After you have deployed your helm chart and pods are in running state, you need to port-forward your app and mysql pod in seperate terminals in order to connect since we have deployed it in a minikube cluster using NodePort. Use below commands:
kubectl port-forward svc/docker-mysql 32762:3306
kubectl port-forward svc/couponservice-couponservice-chart 32763:9091
9. After this you can connect to your mysql db using DBeaver or MySql Workbench using above url and run below SQL commands one by one:
use mydb
select * from coupon 
insert into coupon (code,discount,exp_date) values('SUPERSALE',10,'22-22-2024')
10. At last you can try to hit http://127.0.0.1:32763/couponapi/coupons/SUPERSALE and this will give you a response if everything is working fine.