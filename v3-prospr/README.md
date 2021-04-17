#Prospr Protein Predictor
## Build Docker Image

```bash
>docker build -t prospr_jupyter .
```

## Run Image with Volume
```bash
>sudo docker run -d -p 8888:8888 --gpus all --ipc=host -v <path to data dictory>:/predictor/data prospr_jupyter jupyter notebook --no-browser --ip=0.0.0.0 --allow-root --NotebookApp.token= --notebook-dir='/predictor'
```
