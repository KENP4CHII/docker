#	Docker Kurulum ve Temel Kavramlar:
 ## Docker'ı Ubuntu üzerinde nasıl kurarsınız ve çalışır duruma getirirsiniz? 
 
 - Docker'ın apt deposunu kurun.

 Docker'ın resmi GPG anahtarını ekleyin:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

- Docker paketlerini yükleyin.
### En Son Özel Sürüm
En son sürümü yüklemek için şunu çalıştırın:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- Merhaba dünya görüntüsünü çalıştırarak Docker Engine kurulumunun başarılı olduğunu doğrulayın.

```bash
 sudo docker run hello-world
```

### Docker Engine için Linux kurulum sonrası adımları
- Docker grubunu oluşturun.
  
```bash
  sudo groupadd docker
 ```
- Kullanıcınızı docker grubuna ekleyin.
```bash
  sudo usermod -aG docker $USER
 ```
 - Gruplarda yapılan değişiklikleri etkinleştirmek için aşağıdaki komutu da çalıştırabilirsiniz:
 ```bash
 newgrp docker
 ```

- Docker komutlarını sudo olmadan çalıştırabildiğinizi doğrulayın.
```bash  
  docker run hello-world
```

 ## Docker konteynerlerinin sanal makinelerden nasıl farklı olduğunu açıklayın.

 ### İzolasyon Seviyesi:
  Docker konteynerleri, işletim sistemi düzeyinde izolasyon sağlar. Yani, bir ana bilgisayarda çalışan Docker konteynerleri, aynı ana bilgisayarın işletim sistemi çekirdeğini paylaşır, ancak izole dosya sistemi, ağ ve işlem a

### Kaynak Kullanımı:

   Docker konteynerleri, daha hafif ve daha hızlı başlatılabilir. Çünkü her bir konteyner, ortak bir işletim sistemi çekirdeğini paylaştığı için, işletim sistemi kaynaklarını paylaşır ve daha az bellek ve işlemci gücü tüketir.
### Taşınabilirlik ve Dağıtım:

Docker konteynerleri, uygulamaları ve bağımlılıkları birleştirir ve taşınabilir bir şekilde paketler. Bu, yazılımın farklı ortamlara (geliştirme, test, üretim) sorunsuz bir şekilde dağıtılmasını sağlar.

### Performans:

Docker konteynerleri, işletim sistemi düzeyinde çalıştığı için daha yüksek performans sağlar. Konteynerler, sanal makinelerden daha hızlı başlatılabilir ve daha az kaynak tüketir.
 
 ## Docker imajları ve konteynerler arasındaki ilişkiyi açıklayın. 

 - Docker imajları, Docker konteynerlerinin oluşturulması için temel alınan şablonlardır. Konteynerler, imajlarının bir örneğidir.
- Bir Docker imajından birden çok Docker konteyneri oluşturulabilir. Bu konteynerler, aynı imajı paylaşabilir, ancak her biri kendi izole çalışma ortamına sahip olur.
- Docker konteynerleri, Docker imajlarının çalıştırılabilir durumlarıdır. İmajlar, depolama ve dağıtım için kullanılırken, konteynerler gerçek çalışma zamanında iş yürütmek için kullanılır.
 
 ## Docker Hub'dan bir resmi imajı (nginx, mysql, node, vb.) indirin ve yerel olarak çalıştırın. 
 
 ```docker
 docker run --name my-custom-nginx-container -v /host/path/nginx.conf:/etc/nginx/nginx.conf:ro 
 -p 8080:80 -d nginx:latest
```

 ## docker ps, docker images, ve docker container ls -a gibi temel Docker komutlarını kullanarak çalışan konteynerleri ve imajları listeyin.


#	Dockerfile ve Docker Build İşlemleri:

## Bir Dockerfile oluşturarak bir basit web sunucusu (örneğin, nginx) nasıl yapılandırılır?

```docker
web:
  image: nginx
  volumes:
   - ./templates:/etc/nginx/templates
  ports:
   - "8080:80"
  environment:
   - NGINX_HOST=foobar.com
   - NGINX_PORT=80
```

## Dockerfile'da kullanılan temel komutları (FROM, RUN, COPY, EXPOSE, CMD) açıklayın. 

- FROM komutu, Docker imajının temelini belirler.
- RUN komutu, Docker imajı oluşturulurken çalıştırılacak komutları belirtir.
- COPY ve ADD komutları, dosyaları Docker imajına kopyalamak için kullanılır.
- EXPOSE komutu, Docker konteyneri tarafından dinlenecek bağlantı noktalarını belirtir.
- CMD komutu, Docker konteyneri başlatıldığında çalıştırılacak varsayılan komutu belirtir.

## Dockerfile'ı kullanarak yerel bir dizinden bir uygulamayı nasıl konteynerize edersiniz? 

## Oluşturulan Docker imajını (docker build) adlandırın ve bir etiketle (tag) etkin. 

``` docker
 docker build -t myapp:latest .
```

## Yapılan değişikliklerle güncellenen Docker imajını (docker build) yeniden oluşturun ve yerel imajlar listesini güncelleyin.

### Dockerfile'ı ve uygulama kodunu güncelleyin.

Dockerfile'daki RUN, COPY veya ADD komutlarını güncelleyebilirsiniz. Uygulama kodunda yapılan değişiklikleri de bu adımda uygulayın.

### Docker imajını yeniden oluşturun.

Terminal veya komut istemcisinde, Dockerfile'ı bulunduğu dizine gidin ve aşağıdaki komutu kullanarak Docker imajını yeniden oluşturun:

```docker
docker build -t myapp:latest
```

Bu komut, Dockerfile'ı ve uygulama kodunu kullanarak myapp adlı Docker imajını yeniden oluşturur ve latest etiketiyle etiketler.

### Yerel imajlar listesini güncelleyin.

Aşağıdaki komutu kullanarak Docker imajlarının güncel listesini görüntüleyebilirsiniz:

```docker
docker images
```

#	Docker Ağ Yapılandırması: 

## Docker'ın varsayılan ağ yapısını açıklayın. 

### Bridge Ağı:

Docker konteynerleri, varsayılan olarak "bridge" adlı bir ağda çalışır. Bu, Docker konteynerlerinin birbirleriyle iletişim kurabilmesini sağlar.

### Bridge Ağı Özellikleri:

Bridge ağı, Docker konteynerlerine IP adresi atanmasını sağlar. Bu sayede konteynerler birbirleriyle ve ana bilgisayarla iletişim kurabilir.

### Dış Erişim:

Docker konteynerleri, bridge ağı aracılığıyla birbirleriyle iletişim kurabilir ve dış dünya ile iletişim kurabilir.

### Host Modu:

Docker konteynerlerinin bridge ağı dışında çalışmasını sağlayan bir alternatif mod olan "host" modu vardır. Host modunda, Docker konteynerleri ana bilgisayarın ağına doğrudan bağlanır ve ağlarını paylaşırlar.

## docker network create komutunu kullanarak yeni bir Docker ağı oluşturun. 

```docker
docker network create mynetwork
```

## Bir Docker konteynerini belirli bir ağa (--network flag) bağlayın ve bu konteynerin diğer konteynerlerle iletişim kurabilmesini sağlayın. 

```docker
docker network create mynetwork
docker run -d --name mycontainer --network mynetwork myimage
```

## Docker konteynerlerinin IP adreslerini ve ağ bilgilerini görüntüleyin.
```docker
docker inspect <container_id>
```

## Bir Docker ağında çalışan konteynerler arasında iletişim kurun ve birbirlerine nasıl erişebileceklerini test edin.

```docker
docker network create mynetwork
docker run -d --name container1 --network mynetwork myimage1
docker run -d --name container2 --network mynetwork myimage2
```

#	Docker Hacim Yönetimi: 

## Docker volüm oluşturma ve bağlama komutlarını (docker volume create, docker volume ls, docker volume rm) kullanarak bir Docker hacmi oluşturun. 


## Bir Docker konteynerini bir Docker hacmiyle nasıl bağlarsınız?

```docker
docker volume create myvolume
docker run -d --name mycontainer -v myvolume:/path/in/container myimage
```


## Bir Docker hacmini kullanarak veri kalıcılığını sağlamak için bir veritabanı (örneğin, MySQL) nasıl çalıştırırsınız? 

```docker
docker run -d --name mysqldb -e MYSQL_ROOT_PASSWORD=my-secret-pw -v mysql_data:/var/lib/mysql mysql:latest
```

## Docker hacimlerinin neden önemli olduğunu ve ne zaman kullanılması gerektiğini açıklayın.

Docker konteynerleri, varsayılan olarak geçici bir yapıya sahiptir. Yani bir konteyner durdurulduğunda veya silindiğinde, içindeki veriler de silinir. Docker hacimleri, konteynerlerin durumları ne olursa olsun verilerin kalıcı olmasını sağlar. Bu, uygulamaların veri kaybı riskini azaltır ve verilerin kalıcılığını sağlar.



## Bir Docker konteynerini silerken bağlı hacimlerin ne olacağını açıklayın ve nasıl yönetilmesi gerektiğini belirtin.

- Docker, konteyneri silerken bağlı hacimleri varsayılan olarak silmez. Bu, veri kalıcılığını sağlamak ve veri kaybını önlemek için önemlidir. Ancak, bağlı hacimler artık kullanılmıyorsa ve atıl durumdaysa, bunları temizlemek gerekebilir.

- Bağlı hacimlerin temizlenmesi öncesi dikkatli olunmalıdır. Hacimleri silmeden önce, bunların gerçekten atıl durumda olduğundan emin olun. Kullanılmayan bir hacmi silmek, veri kaybına neden olabilir, bu nedenle hacimlerin silinmesinden önce gerektiği şekilde yedeklenmesi önemlidir.

#	Docker Kompozisyon Araçları ve Orkestrasyon:

## Docker Compose'un ne olduğunu ve temel kullanım senaryolarını açıklayın. 

- YAML Dosyaları ile Tanımlama
- Çoklu Konteyner Yönetimi
- Geliştirme Ortamları için Kolaylık
- Tek Komutla Uygulama Yönetimi
- Üretim Ortamları için Yapılandırma

## Bir Docker Compose dosyası oluşturun ve birden fazla konteynerin (örneğin, web sunucusu ve veritabanı) nasıl başlatılacağını tanımlayın. 

```docker
version: '3.8'

services:
  webserver:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    networks:
      - mynetwork

  database:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: my-secret-pw
      MYSQL_DATABASE: my_database
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - mynetwork

networks:
  mynetwork:

volumes:
  mysql_data:
```

## Docker Compose ile birden fazla konteyneri başlatın, durdurun ve yeniden başlatın. 

```docker
docker-compose up -d
docker-compose down
docker-compose up -d
```

## Bir Docker Swarm kümesi oluşturun ve konteynerleri bu kümede nasıl dağıtabileceğinizi açıklayın. 

```docker
docker swarm init --advertise-addr <manager_ip>
docker service create --name my-webserver --replicas 3 -p 80:80 nginx
docker service ls
```

## Kubernetes üzerinde bir Docker konteynerinin nasıl dağıtıldığını ve yönetildiğini anlatın.
