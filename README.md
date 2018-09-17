# Bug report

Run the following commands:

```
$ ansible-playbook -i inventory playbook.yml --diff
```

The playbook will crash with the following error:

```
~/repos/channable/ansible-bug-report master*
devops ❯ ansible-playbook -i inventory playbook.yml --diff

PLAY [localhost]
***************************************************************************************

TASK [Gathering Facts]
*********************************************************************************
ok: [localhost]

TASK [problem : copy problematic file]
*****************************************************************
--- before
+++ after:
/home/laurens/repos/channable/ansible-bug-report/roles/problem/files/problem.yml
@@ -0,0 +1,7 @@
+The problem was originally found when copying prometheus
+templates, which can contain things like {{ $labels.instance }}.
+
+If this were a file in the `templates/` directory, I'd agree
+that this is asking for problems. However, this only gives
+problems when registering things to a variable and acting on
+it.

changed: [localhost]

TASK [problem : inspect results]
***********************************************************************
fatal: [localhost]: FAILED! => {"msg": "An unhandled exception occurred while
templating '{'diff': [{'before': '', 'after_header':
'/home/laurens/repos/channable/ansible-bug-report/roles/problem/files/problem.yml',
'after': \"The problem was originally found when copying
prometheus\\ntemplates, which can contain things like {{ $labels.instance
}}.\\n\\nIf this were a file in the `templates/` directory, I'd agree\\nthat
this is asking for problems. However, this only gives\\nproblems when
registering things to a variable and acting on\\nit.\\n\"}], 'src':
'/home/laurens/.ansible/tmp/ansible-tmp-1537170680.4934566-139695046702738/source',
'changed': True, 'group': 'laurens', 'uid': 1000, 'dest': '/tmp/problem.yml',
'checksum': '42a9a3edeb77bac53775ea092fab3cad962c0872', 'md5sum':
'6e6ce6086486a66f1cdab91b98d0b7f0', 'owner': 'laurens', 'state': 'file', 'gid':
1000, 'mode': '0664', 'size': 308, 'failed': False}'. Error was a <class
'ansible.errors.AnsibleError'>, original message: template error while
templating string: unexpected char '$' at 101. String: The problem was
originally found when copying prometheus\ntemplates, which can contain things
like {{ $labels.instance }}.\n\nIf this were a file in the `templates/`
directory, I'd agree\nthat this is asking for problems. However, this only
gives\nproblems when registering things to a variable and acting on\nit.\n"}
to retry, use: --limit
@/home/laurens/repos/channable/ansible-bug-report/playbook.retry

PLAY RECAP
*********************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0
failed=1
```

If `--diff` is omitted, this task runs fine.
