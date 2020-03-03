.PHONY: clean test lint init

JOBS ?= 1

help:
	@echo "make"
	@echo "    clean"
	@echo "        Remove Python/build artifacts."
	@echo "    install"
	@echo "        Install {{cookiecutter.project_slug}}."
	@echo "    formatter"
	@echo "        Apply black formatting to code."
	@echo "    lint"
	@echo "        Lint code with flake8, and check if black formatter should be applied."
	@echo "    types"
	@echo "        Check for type errors using pytype."
	@echo "    test"
	@echo "        Run pytest on tests/."
	@echo "        Use the JOBS environment variable to configure number of workers (default: 1)."
	@echo "    doctest"
	@echo "        Run all doctests embedded in the documentation."
	@echo "    livedocs"
	@echo "        Build the docs locally."

clean:
	find . -name '*.pyc' -exec rm -f {} +
	find . -name '*.pyo' -exec rm -f {} +
	find . -name '*~' -exec rm -f  {} +
	rm -rf build/
	rm -rf .pytype/
	rm -rf dist/
	rm -rf docs/_build

install:
	poetry run python -m pip install -U 'pip<20'
	poetry install -E full

formatter:
	poetry run black {{cookiecutter.project_slug}} tests

lint:
	poetry run flake8 {{cookiecutter.project_slug}} tests
	poetry run black --check {{cookiecutter.project_slug}} tests

types:
	poetry run pytype --keep-going {{cookiecutter.project_slug}} -j 16
test: clean
	# OMP_NUM_THREADS can improve overral performance using one thread by process (on tensorflow), avoiding overload
	OMP_NUM_THREADS=1 poetry run coverage pytest tests -n $(JOBS) --cov {{cookiecutter.project_slug}}

doctest: clean
	cd docs && poetry run make doctest

livedocs:
	cd docs && poetry run make livehtml
