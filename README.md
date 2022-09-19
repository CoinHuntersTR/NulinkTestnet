# NuLink Testnet Rehberi

<img alt="" src="https://miro.medium.com/max/700/1*AyE-4PKhVnQ5ZKmzkVN0sQ.png">



### Sistem Gereksinimleri 

|CPU | RAM  | Disk  | 
|----|------|----------|
|   4| 4GB  | 30GB     |

 # Daha önce Node kurulumu yapmadıysanız buradan sırasıyla adımları takip ederek tüm süreci öğrenebilirsiniz.
  ## Yeni Başladım Rehberi; [Pusula Finans Labs.](https://www.labs.pusulafinans.com/category/rehber/)
  ### 1. [Testnet ve Node test kurulum rehberi Bölüm-1](https://www.labs.pusulafinans.com/2022/08/23/testnet-ve-node-kurulum-rehberi/)
  ### 2. [Yeni Chrome Tarayıcı nasıl açarım?](https://www.labs.pusulafinans.com/2022/08/23/yeni-chrome-tarayici-nasil-acarim/)
  ### 3. [Ücretsiz Sunucu Kiralama](https://www.labs.pusulafinans.com/2022/08/23/nasil-ucretsiz-sunucu-kiralarim/)
  ### 4. [Digital Ocean Nasıl Kayıt olurum?](https://www.labs.pusulafinans.com/2022/08/23/digital-oceana-nasil-kayit-olabilirim/)
  ### 5. [MobaXTerm Terminal Kurulumu](https://www.labs.pusulafinans.com/2022/08/23/mobaxterm-terminal-kurulumu/)
  
# Kuruluma başlayalım.

#### İlk önce 9151 portuna izin verelim;

```
sudo ufw allow 9151
```

### Güncellemeleri yapalım

```
sudo apt update && sudo apt upgrade -y
```

```
sudo apt-get -y install libssl-dev && apt-get -y install cmake build-essential git wget jq make gcc
```

#### kodu girelim şifre yazalım 
```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.24-972007a5.tar.gz

tar -xvzf geth-linux-amd64-1.10.24-972007a5.tar.gz

cd geth-linux-amd64-1.10.24-972007a5/

./geth account new --keystore ./keystore
```
#### bize görseldeki gibi keyler verecek bunları not defterine kaydedelim

<img alt="" src="https://miro.medium.com/max/700/1*u3LqSi9yO7dUSFT1kNmnEw.png">

#### Sırasıyla adımları takip edelim.
```
cd /root
```

```
sudo apt install docker.io -y
```

```
sudo systemctl enable --now docker
```

```
docker pull nulink/nulink:latest
```

```
cd /root
mkdir nulink
```

#### Path of the secret key file bölümünde çıkan uzantıyı komple "Bukısmagelecek" yere ekliyoruz Altaki görselde hangi kısmı alacağınızı gösterdik.
```
cp Bukısmagelecek /root/nulink
```
<img alt="" src="https://miro.medium.com/max/700/1*wfmG9N9iyyPX8RDD1Lc3tw.png">

```
chmod -R 777 /root/nulink
```

#### Buraya en az 8 haneli şifre girin ikisinide aynı yapın.

```
export NULINK_KEYSTORE_PASSWORD=SIFREBELİRLE

export NULINK_OPERATOR_ETH_PASSWORD=AYNISIFREYIYAZ
```

####  Aşağıdaki kodu giriyoruz. Kod içerisinde; "UTCkısmıekle" ve "PUBLICadres" kısımlarını değiştirmeyi unutmayın.

```
docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/UTC dahilkısmıekle \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545  \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--payment-network bsc_testnet \
--operator-address PUBLICadres \
--max-gas-price 100
```
UTC Dahil adres için bu görsele bakabilirsiniz;
<img alt="" src="https://miro.medium.com/max/2400/1*nMOg2Ahs-PWBxPW45HtpzA.png">
Public adres için bu görsele bakarak yerini bulabilirsiniz.
<img alt="" src="https://miro.medium.com/max/2400/1*E1mMXPDgi-plf0s2uRuY3A.png">

# ÖNEMLİ
Yukarıdaki kodu girdikten sonra size ekranda;
<img alt="" src="https://miro.medium.com/max/2400/1*W6vDkQCYDgF2k2YPNsHJmg.png">
bu şekilde uyarı gelecek ve sadece Y harfine ve enter'a bas.

# !Size anahtar kelimeleri verecek bunları bir yere not etmeden işleme devam etmeyin; Görseldeki gibi Ctrl+C işlemi yapmadan ekran resmi ile kayıt edip bir yere not edebilirsiniz.
<img alt="" src="https://miro.medium.com/max/2400/1*VBu0LdSoDZbWV2yvfWluMg.png">

## y Enter yapıyoruz, Biraz önce verilen kelimeleri sırasıyla tekrar giriyoruz.

## Bu işlem Sonrasında size aşağıdakine benzer bir çıktı verecek bunu bir yere not etmeyi unutmayın;
<img alt="" src="https://miro.medium.com/max/2400/1*inlEXOZ0VJm2VDZSmdblGw.png">

## Node Başlatıyoruz:
```
docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready
```

#### Screen açarak logların doğru şekilde aktığını kontrol edelim.
```
apt install screen
```

```
screen -S log
```

```
docker logs -f ursula
```
#### Bu şekilde çıktı aldığınızda işlemleri doğru şekilde yapmışsınız demektir
<img alt="" src="https://miro.medium.com/max/2400/1*Y9SwHoPZCrZiOz-STTIRdQ.png">

## Bu işlemlerden Sonra Bölüm-3 Devam ediyoruz. [Buradan](https://github.com/pusulafinanslabs/NulinkTestnet/blob/main/Bolum3.md) Bölüm-3 ulaşabilirsiniz.





