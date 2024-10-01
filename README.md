# Django Configuração de Projeto Padrão (Simples)

Essa configuração simples de projeto django que utilizo para fazer testes e criar aplicações locais.

Espero que ajude !!!

Esse é link do Vídeo Tutorial [Link](https://www.youtube.com/watch?v=0y5YdiK7x0k)

**Configurações Iniciais**

<details><summary><b>Ambiente Virtual Linux/Windows</b></summary>

- **Ambiente Virtual Linux/Windows**
    
    
    Lembrando… Precisa ter Python instalado no seu ambiente.
    
    **Criar o ambiente virtual Linux/Windows**
    
    ```python
    ## Windows
    python -m venv .venv
    source .venv/Scripts/activate # Ativar ambiente
    
    ## Linux 
    ## Caso não tenha virtualenv. "pip install virtualenv"
    virtualenv .venv
    source .venv/bin/activate # Ativar ambiente
    ```
    
    Instalar os seguintes pacotes.
    
    ```python
    # Instala o Django na ultima versão
    # esta sendo colocado o "3" depois do pip e do python, pois se refere ao Python 3
    pip3 install django
    # Quando quiser instalar uma versão especifica
    python3 -m pip3 install Django==5.1.1
    # O "-m" o Python irá reconhece o que tiver depois dele como Script e executará.
    
    # Manipulador de imagens
    pip3 install pillow
    ```
    
    Para criar o arquivo *requirements.txt*
    
    ```python
    # Esse arquivo guarda as configuração, facilitando a Reinstalação caso necessario.
    pip3 freeze > requirements.txt
    
    # Comando para fazer a Reintalação das dependencias.
    pip3 install -r requirements.txt
    ```

</details>

<details><summary><b>Criando o Projeto</b></summary>

- **Criando o Projeto**
    
    ## **Criando o projeto**
    
    *__“core”__* é nome do seu projeto e quando colocamos um __*“.”*__ depois do nome do projeto significa que é para criar os arquivos na raiz da pasta. Assim não cria subpasta do projeto.
    
    ```python
    django-admin startproject core .
    ```
    
    **Testar a aplicação**
    
    ```python
    python3 manage.py runserver
    ```
    
</details>

<details><summary><b>Configurar Settings e Arquivos Static</b></summary>

- **Configurar Settings e Arquivos Static**
    
    ## **Vamos configurar nossos arquivos** *static*
    
    ```python
    # Modulo "os" fornece funções para interagir com o sistema operacional.
    import os 
    
    # base_dir config
    # Use isso quando o Projeto for ser usado em ambiente com Python anterior ao 3.4
    BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
    TEMPLATE_DIR = os.path.join(BASE_DIR,'templates')
    STATIC_DIR=os.path.join(BASE_DIR,'static')
    
    # Caso prefire usar o base_dir padrão do Django ficaria assim
    BASE_DIR = Path(__file__).resolve().parent.parent
    TEMPLATE_DIR = BASE_DIR / 'templates'
    STATIC_DIR = BASE_DIR / 'static'
    
    # Database
    # O formato padrão do Django aparentemente fará a mesma coisa
    # Por via das duvidas só use quando modificar a "base_dir"
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'), 
        }
    }
    
    
    # Essa Abordagem tem vantagem no desenvolvimento.
    # Tanto no "STATIC" quanto no "MEDIA", teram rotas bem definidas
    # Mais essa configuração abaixo só use quando modificar o "base_dir"
    STATIC_ROOT = os.path.join(BASE_DIR,'static')
    STATIC_URL = '/static/' 
    
    MEDIA_ROOT=os.path.join(BASE_DIR,'media')
    MEDIA_URL = '/media/'
    
    # Caso use a configuração padrão do "base_dir"
    STATIC_URL = '/static/'
    STATIC_ROOT = BASE_DIR / 'static'

    MEDIA_URL = '/media/'
    MEDIA_ROOT = BASE_DIR / 'media'
    
    
    # Internationalization
    # Se quiser deixar em PT BR
    LANGUAGE_CODE = 'pt-br'
    TIME_ZONE = 'America/Sao_Paulo'
    USE_I18N = True
    USE_L10N = True 
    USE_TZ = True
    ```
    
    *myapp/urls.py* **(Isso não é influenciado pelo "base_dir")**
    
    ```python
    from django.contrib import admin
    from django.conf import settings
    from django.conf.urls.static import static
    from django.urls import path
    
    urlpatterns = [
        path('admin/', admin.site.urls),
    ]
    
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```
</details>

<details><summary><b>Criando Aplicativo</b></summary>

- **Criando Aplicativo**
    
    **Vamos criar nosso aplicativo no Django.**
    
    Para criar a aplicação no Django rode comando abaixo. “myapp” é nome do seu App.
    
    ```python
    python3 manage.py startapp myapp
    ```
    
    Agora precisamos registrar nossa aplicação no *INSTALLED_APPS* localizado em *settings.py*.
    
</details>

<details><summary><b>Template base e Bootstrap Configuração</b></summary>

- **Template base e Bootstrap Configuração**
    
    ### Bootstrap configuração
    
    Doc: [https://getbootstrap.com/docs/5.2/getting-started/introduction/](https://getbootstrap.com/docs/5.2/getting-started/introduction/)
    
    Com Base na documentação para utilizar os recursos Boostrap basta adicionar as tags de CSS e JS. No HTML da Pagina Base.
    
    ```python
    <!-- CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
    
    <!-- JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4" crossorigin="anonymous"></script>
    ```
    
    ## Template Base
    
    0 - Criar a pasta _**"templates"**_, devido a configuração anteriores pode se colocar essa pasta onde quise
    Mais por questões de organização crie ela dentro do diretorio do App.
    ***    
    1 - Criar um arquivo base ***base.html*** onde vamos renderizar nosso conteúdo. 
    Com a utilização do Bootstrap usando um template base, vai manter a consistencia entre as diferente paginas do projeto.
    
    ```python
    {% load static %}
    <!DOCTYPE html>
    <html lang="en">
    <head>
    	<meta charset="UTF-8">
    	<meta http-equiv="X-UA-Compatible" content="IE=edge">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>{% block title %}{% endblock %}</title>
    
    	<!-- CSS -->
    	<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
    
    </head>
    <body>  
    	
    	{% block content %}
    	
    	{% endblock %} 
     
    	<!-- JS-->
    	<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4" crossorigin="anonymous"></script>
    </body>
    </html>
    ```

</details>

<details><summary><b>Cria uma View</b></summary>

- **Cria uma View**
    
    *index.html*
    A Criação do index.html tem que ser dentro do diretorio templates
    
    ```html
    
    {% extends 'base.html' %}
    
    {% block title %}Pagina Inicial{% endblock %}
    
    
    {% block content %}
    	<h1>Pagina Inicial</h1>
    {% endblock %}
    ```
    
    Dentro do arquivo "views.py" do App
    *myapp/views.py*
    Crie a função abaixo o nome "mysite" pode ser auterado
    Mais lembre-se no decorrer do projeto.
    
    ```python
    from django.shortcuts import render
    
    # Create your views here.
    def mysite(request):
        return render(request, 'index.html')
    ```
    
    Dentro do diretorio do App crie o arquivo "urls.py".
    criar arquivo *myapp*/*urls.py*
    E no 'name' utilize o mesmo nome da função de criou no views.py do App
    
    ```
    from django.urls import path 
    from myapp import views
    
    urlpatterns = [
        path('', views.mysite, name='mysite'), 
    ]
    ```
    
    urls.py do projeto. ***core/urls.py***
    O arquivo urls.py criado no App precisa ser referenciado aqui
    
    ```python
    from django.contrib import admin
    from django.urls import path, include # adicionar include
    from django.conf import settings
    from django.conf.urls.static import static 
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('myapp.urls')), # url do app
    ]
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT) # Adicionar Isto
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) # Adicionar Isto
    ```
    
    Rodar o projeto para ver como está.
    
    ```python
    python manage.py makemigrations && python manage.py migrate
    python manage.py runserver
    ```
    
    .gitignore
    A criação desse arquivo com esse parametros abaixo vai fazer o git ignorar esse itens.
    Precisa ser criado dentri da Raiz, no mesmo diretorio que o Projeto foi criado.
    
    ```python
    /tmp
    passenger_wsgi.py 
    .venv
    db.sqlite3
    /static_media
    static_media
    /static_files
    static_files
    /media
    mydatabase
    file_name.sql
    __pycache__ 
    __pycache__/
    ```

</details>
