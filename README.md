# demo-gitops

Implementing OpenShift GitOps with FLUX


```bash
dnf install -y epel-release
dnf install -y snapd
ln -s /var/lib/snapd/snap /snap
systemctl enable snapd
systemctl start snapd
snap install fluxctl --classic

export FLUX_FORWARD_NAMESPACE=flux
fluxctl list-workloads

export KUBECONFIG=/root/okd-config/auth/kubeconfig
oc new-project flux
oc create ns flux

export GHUSER="YOUR GITHUB USERNAME"
fluxctl install --add-security-context=false \
  --git-user=${GHUSER} \
  --git-email=${GHUSER}@users.noreply.github.com \
  --git-url=git@github.com:${GHUSER}/demo-gitops \
  --git-branch=main \
  --namespace=flux | oc apply -f -

fluxctl identity
```

Copy output from `fluxctl identity` to **Repository** > **Settings** > **Deploy Keys**

- Give Title
- Copy Key
- Check **Allow write access**
- Click Add key

```bash
fluxctl sync # synchronize the cluster with the git repository, now

fluxctl list-workloads -n gitops-demo
```
