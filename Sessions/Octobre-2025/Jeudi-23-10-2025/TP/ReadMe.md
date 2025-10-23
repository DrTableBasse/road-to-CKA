# Avec Kind — Découverte de kubectl

Manipuler un cluster Kind pour découvrir les bases de kubectl.

## Pré-requis
- Un cluster Kind démarré
- kubectl configuré pour communiquer avec ce cluster

---

## Questions / Exercices

### 1) Explorer le cluster
Tu as un cluster Kind. Réponds à ces 3 questions :

a) Comment lister tous les nodes de ton cluster ?  
<details>
<summary>Afficher la Réponse</summary>

```bash
kubectl get nodes
```

</details>

b) Quel est le nom du node "control-plane" ?  

<details>
<summary>Afficher la Réponse</summary>

```bash
kubectl get nodes -o wide
# ou
kubectl get nodes -o name
```
</details>


c) Combien de nodes as-tu ?  

<details>
<summary>Afficher la Réponse</summary>
```bash
kubectl get nodes --no-headers | wc -l
```
</details>


Indice : cherche la commande pour lister les "nodes".

---

### 2) Namespace actuel
Dans quel namespace travailles-tu par défaut ?  


<details>
<summary>Afficher la Réponse</summary>
```bash
kubectl config view --minify --output 'jsonpath={..namespace}'
```
(Note : si la sortie est vide, le namespace par défaut est `default`.)

</details>

Indice : `kubectl config --help` peut aider.

---

### 3) Explorer kube-system
Liste tous les pods du namespace `kube-system` :


<details>
<summary>Afficher la Réponse</summary>
```bash
kubectl get pods -n kube-system
```
</details>
- Combien y en a-t-il ? → compte les lignes du résultat.
- Quel est le rôle du pod `etcd-*` ?  
    Réponse courte : etcd est la base de données clé-valeur du control plane — il stocke l'état du cluster Kubernetes.

---

### 4) Créer un pod
Crée un pod nommé `test-nginx` avec l'image `nginx:alpine`.  
Commande (créera un Pod, pas un Deployment) :

<details>
<summary>Afficher la Réponse</summary>
```bash
kubectl run test-nginx --image=nginx:alpine --restart=Never
```
</details>
---

### 5) Dry-run YAML
Supprime le pod précédent :

<details>
<summary>Afficher la Réponse</summary>
```bash
kubectl delete pod test-nginx
```
</details>

Recrée-le SANS le lancer, génère le YAML et sauvegarde-le dans `pod.yaml`.  
<details>
<summary>Afficher la Réponse</summary>
```bash
kubectl run test-nginx --image=nginx:alpine --restart=Never --dry-run=client -o yaml > pod.yaml
```
</details>
(Option importante : `--dry-run=client` et `-o yaml`.)

---

### 6) Déployer depuis YAML
Déploie le pod à partir de `pod.yaml` :

<details>
<summary>Afficher la Réponse</summary>
```bash
kubectl apply -f pod.yaml
# ou
kubectl create -f pod.yaml
```
</details>

Petit plus — Essayez d'explorer les logs :

<details>
<summary>Afficher la Réponse</summary>
```bash
kubectl logs test-nginx
kubectl describe pod test-nginx
# si nécessaire, préciser le namespace :
kubectl logs test-nginx -n <namespace>
kubectl describe pod test-nginx -n <namespace>
```
</details>
---

Bon exercice — explore les variations (ex: `-o wide`, `-n`, `--selector`) pour te familiariser avec kubectl.