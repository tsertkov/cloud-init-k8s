VM ?= centos
PROJECT_DIR = $(dir $(abspath $(lastword $(MAKEFILE_LIST))))
VM_DIR = $(PROJECT_DIR)$(VM)
KUBECONFIG = $(VM_DIR)/kube-config
KUBECTL = kubectl --kubeconfig="$(KUBECONFIG)"
VAGRANT = VAGRANT_CWD="$(VM_DIR)" vagrant

nocloud.iso : $(VM_DIR)/meta-data $(VM_DIR)/user-data
	@echo "---"
	@echo "[up] Building cloud-init iso:"
	@echo "---"
	mkisofs \
		-joliet -rock \
		-volid "cidata" \
		-output "$(VM_DIR)/nocloud.iso" "$(VM_DIR)/meta-data" "$(VM_DIR)/user-data"

up : nocloud.iso
	@echo "---"
	@echo "[up] Bringing VM up:"
	@echo "---"
	$(VAGRANT) up

	@echo "---"
	@echo "[up] Watching cloud-init-outpout.log (Ctrl+C to exit):"
	@echo "---"
	$(VAGRANT) ssh -- tail -n+0 -f /var/log/cloud-init-output.log

stop :
	@echo "---"
	@echo "[stop] Shutting down VM:"
	@echo "---"
	$(VAGRANT) halt

kubeconfig :
	@echo "---"
	@echo "[copy-config] Copying kubernetes config file from vm to host:"
	@echo "---"
	$(VAGRANT) ssh -- sudo cat /etc/kubernetes/admin.conf > $(KUBECONFIG)

destroy :
	@echo "---"
	@echo "[destroy] Destroying VM:"
	@echo "---"
	$(VAGRANT) destroy -f

dashboard :
	@echo "---"
	@echo "[dashboard] Opening kubernetes dashboard:"
	@echo "---"
	open http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
	$(KUBECTL) proxy --port=8001

copy-token :
	@echo "---"
	@echo "[copy-token] Copy superuser token to clipboard:"
	@echo "---"
	$(KUBECTL) get serviceAccount superuser -n kube-system -o jsonpath='{.secrets[0].name}' \
	  | xargs -I {} kubectl get secret {} -n kube-system -o jsonpath='{.data.token}' \
		| base64 -D | pbcopy

hello-world :
	@echo "---"
	@echo "[hello-world] Deploying hello-world app:"
	@echo "---"
	$(KUBECTL) run node-hello --image=gcr.io/google-samples/node-hello:1.0 --port=8080 || echo

hello-world-proxy:
	@echo "---"
	@echo "[hello-world-proxy] Proxy hello-world app to localhost:8080:"
	@echo "---"
	$(KUBECTL) get pods -l run=node-hello -o jsonpath='{.items[0].metadata.name}' \
		| xargs -I {} $(KUBECTL) port-forward {} 8080:8080

kubectl-env :
	@echo export KUBECONFIG="$(KUBECONFIG)"

vagrant-env :
	@echo export VAGRANT_CWD="$(VM_DIR)"
