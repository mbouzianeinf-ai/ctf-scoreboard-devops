# TP3 answers

## Exercise 1 (Bouziane Chakib)

**(a)** `scoreboard:local` is 181 MB and `python:3.12-slim` is 119 MB. The application adds about 62 MB (the virtual environment with its dependencies).

**(b)** Two things are in the build stage but not in the runtime image:
1. `requirements.txt`, `pyproject.toml` and `README.md` (the build files used to install the app).
2. The `src/` source folder. The final `/app` contains only `data/`, and the installed package lives in `/opt/venv`.

**(c)**
- Without `requirements.txt` and `pyproject.toml`, an attacker inside the container cannot easily read the exact dependency list and versions to look for known vulnerabilities.
- Without the `src/` copy, there is no source tree to read, modify or reuse for a rebuild.
- In general, a smaller image has a smaller attack surface: every file that is not needed at runtime is a file an attacker can use.

**(d)** Docker caches each step in order, and when one step changes, all the steps after it run again. Dependencies change rarely and code changes often, so `COPY requirements.txt` and `pip install` come first. After I changed one line of `app.py`, the second build printed:

    CACHED [build 2/8] WORKDIR /app
    CACHED [build 3/8] RUN python -m venv /opt/venv
    CACHED [build 4/8] COPY requirements.txt .
    CACHED [build 5/8] RUN pip install --no-cache-dir -r requirements.txt
    CACHED [build 6/8] COPY pyproject.toml README.md ./

Only `COPY src/` and the steps after it ran again. The build took 7.9 s instead of 29.3 s, because the slow dependency install was reused. If `COPY src/` came first, every code change would reinstall all the dependencies.

## Exercise 2

## Exercise 3

## Exercise 4

## Exercise 5
