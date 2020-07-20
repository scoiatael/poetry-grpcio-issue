# Issue with Poetry 1.1.0a3 and Python 3.6

Looks like when using new Poetry resolver there's a problem installing pre-built wheels like `grpcio`:

``` sh
$ docker build -f Dockerfile.0.9 .
...
Step 6/6 : RUN poetry install
 ---> Running in ae6f67e691a1
Creating virtualenv poetry-grpcio-issue-o9msT97p-py3.6 in /root/.cache/pypoetry/virtualenvs
Updating dependencies
Resolving dependencies...

Writing lock file


Package operations: 2 installs, 0 updates, 0 removals

  - Installing six (1.15.0)
  - Installing grpcio (1.30.0)
Removing intermediate container ae6f67e691a1
 ---> 9c5aa18bb60e
Successfully built 9c5aa18bb60e
```

``` sh
$ docker build -f Dockerfile.1.0a3 .
...
Step 6/6 : RUN poetry install
 ---> Running in 2d9180a8c94b
Creating virtualenv poetry-grpcio-issue-o9msT97p-py3.6 in /root/.cache/pypoetry/virtualenvs
Updating dependencies
Resolving dependencies...

Writing lock file

Package operations: 2 installs, 0 updates, 0 removals

  • Installing six (1.15.0)
  • Installing grpcio (1.30.0)

  EnvCommandError

  Command ['/root/.cache/pypoetry/virtualenvs/poetry-grpcio-issue-o9msT97p-py3.6/bin/python', '-m', 'pip', 'install', '--no-deps', '/root/.cache/pypoetry/artifacts/e4/1a/fb/cc14443ebcdd3b0d5df7ca7fbffdc7d9343564703a67906925174cd179/grpcio-1.30.0-cp36-cp36m-manylinux2010_x86_64.whl'] errored with the following return code 1, and output:
  grpcio-1.30.0-cp36-cp36m-manylinux2010_x86_64.whl is not a supported wheel on this platform.
  You are using pip version 18.1, however version 20.2b1 is available.
  You should consider upgrading via the 'pip install --upgrade pip' command.


  at /usr/local/lib/python3.6/site-packages/poetry/utils/env.py:937 in _run
       933│                 output = subprocess.check_output(
       934│                     cmd, stderr=subprocess.STDOUT, **kwargs
       935│                 )
       936│         except CalledProcessError as e:
    →  937│             raise EnvCommandError(e, input=input_)
       938│
       939│         return decode(output)
       940│
       941│     def execute(self, bin, *args, **kwargs):

The command '/bin/sh -c poetry install' returned a non-zero code: 1
```

