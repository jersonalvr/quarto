Instalar Quarto https://quarto.org/docs/get-started/ y la extension de Quarto en VSCode

```bash
sudo apt-get install texlive-full
```

## Clona el repositorio
```bash
gh repo clone jersonalvr/quarto
cd quarto
git checkout Y1018-Y04AN1-2025-2-Big_Data_Aplicada
```

## Creación del entorno
Instalar previamente entornos virtuales `sudo apt install python3-venv python3-full` y extension de Python

```bash
source spark_env/bin/activate
python3 -m ipykernel install --user --name=spark_env
```
   - Presionar `Ctrl+Shift+P`
   - Escribir "Python: Select Interpreter" o "Jupyter: Select Kernel"
   - Seleccionar el kernel "spark_env"

