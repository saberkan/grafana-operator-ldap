# grafana-operator-ldap

This lab is an exemple of grafana operator configuration with ldap in OpenShift 4.5 cluster.

## Step 1: Create grafana and ldap namespaces

## Step 2: Install Grafana operator from operatorhub in grafana namespace

## Step 3: Add role to ldap, and deploy manifests/ldap
<pre>
oc adm policy add-scc-to-user anyuid -z default -n ldap
</pre>

Once resources deployed, Ldap can be accessed with "cn=admin,dc=example,dc=com"/admin credentials:
<pre>
echo https://$(oc get route ldap-admin -n ldap -o jsonpath='{.spec.host}')
</pre>

Please notice that users as erin, vanessa, etc. have password 'admin    ' with 4 escapes.

## Step 4: Create grafana instance from manifests/grafana first, then add role and get token to be used in datasource in order to scrape openshift thanos metrics
<pre>
oc adm policy add-cluster-role-to-user cluster-monitoring-view -z grafana-serviceaccount
oc serviceaccounts get-token grafana-serviceaccount
</pre>

Then place token in grafana-datasource.yaml.


With the following configuration, users can access ldap as Editors, and admin can create teams, and folders directly in grafana to configure users. Meanwhile, notice that grafana instance is stateless.
