# AAP role lab

A small, read-only Linux role project for a first AAP 2.7 job. `site.yml` targets
the `lab_linux` inventory group and calls `roles/system_report`. The role uses
Ansible built-in modules only; the AAP default execution environment is enough.

## Layout

```text
site.yml
roles/system_report/defaults/main.yml
roles/system_report/tasks/main.yml
inventories/lab/hosts.yml
ansible.cfg
```

## Try it locally

The included inventory targets the machine running Ansible. It is for local
validation only; create your actual lab inventory in AAP.

```bash
ansible-playbook --syntax-check site.yml
ansible-playbook site.yml
```

## Push to GitHub

Create an empty repository named `aap-role-lab` in your GitHub account. From
this directory, run the following, replacing `YOUR_USERNAME`:

```bash
git init -b main
git add .
git commit -m "Add AAP role lab"
git remote add origin https://github.com/YOUR_USERNAME/aap-role-lab.git
git push -u origin main
```

For a private repository, configure an AAP **Source Control** credential with
repository access. Never commit tokens or SSH private keys.

## Configure AAP 2.7

1. Under **Access Management**, create or choose an organization for this lab.
2. Under **Automation Execution > Infrastructure > Inventories**, create an
   inventory, add a group called `lab_linux`, and add your Linux host to it.
   Set its host name or `ansible_host` to an address the execution nodes can
   reach. The repository's localhost inventory is not automatically imported.
3. Under **Automation Execution > Infrastructure > Credentials**, create a
   **Machine** credential with the SSH user and key for that host. If using
   privilege escalation for later roles, configure that separately; this
   read-only role does not require it.
4. Under **Automation Execution > Projects**, create `aap-role-lab`. Choose
   **Git**, paste the GitHub clone URL, select `main`, and add a Source Control
   credential only if the repository is private. Save and confirm its sync
   status is successful.
5. Under **Automation Execution > Templates**, create a **Job template**:
   project `aap-role-lab`, playbook `site.yml`, your lab inventory, Machine
   credential, and an available default execution environment. Save it.
6. Launch the template and inspect **Jobs** for the facts and report. Limit
   the template to the intended lab host when the inventory has more hosts.

For a second role, add `roles/<name>/tasks/main.yml`, then include that role in
`site.yml` or add a separate playbook and job template. Review any modifying
tasks and target limits before running them on shared or production servers.
