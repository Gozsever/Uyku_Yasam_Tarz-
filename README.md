# Uyku Bozuklukları ve Yaşam Tarzı
Uyku bozuklukları ve yaşam tarzı arasındaki ilişki, günümüzde giderek artan bir ilgi konusu haline gelmiştir. Özellikle modern yaşamın getirdiği stres, düzensiz çalışma saatleri, teknoloji kullanımı ve hareketsiz yaşam tarzı gibi faktörler, uyku düzeni üzerinde önemli etkilere sahip olabilir. Örneğin, yetersiz fiziksel aktivite ve düzensiz uyku alışkanlıkları, uyku kalitesini olumsuz yönde etkileyebilir ve uyku bozukluklarının ortaya çıkmasına zemin hazırlayabilir.

Düzenli egzersiz yapmak, sağlıklı bir diyet benimsemek ve stres yönetimi tekniklerini uygulamak gibi yaşam tarzı değişiklikleri, uyku kalitesini artırabilir ve uyku bozukluklarının riskini azaltabilir. 

Bu nedenle, bireylerin uyku düzenleri ve yaşam tarzı seçimleri üzerinde bilinçli olmaları ve gerekli önlemleri almaları, genel sağlık ve iyi bir yaşam kalitesi için önemlidir.

# Amaç ve Önem
Uyku düzenleri, fiziksel aktivite, stres düzeyi ve genel sağlık durumları arasındaki ilişkileri incelemek amacıyla kullanılabilir.

Veri seti bizlere, bireylerin uyku düzenleri ve yaşam tarzı seçimlerinin sağlık üzerindeki etkilerini anlamak için zengin bir kaynak sunar. Bireylerin uyku kalitesi ve süresine etki eden faktörleri analiz ederek, daha sağlıklı yaşam tarzı önerileri ve uyku düzenlemeleri yapılabilir. Bu tür çalışmalar, uyku bozuklukları ile mücadele ve genel sağlık iyileştirme stratejileri geliştirmek için önemli veriler sağlayabilir.

# Veri Seti
Bu veri seti, uyku düzenleri, fiziksel aktivite, stres düzeyi ve genel sağlık durumları arasındaki ilişkileri incelemek amacıyla çevrimiçi olarak paylaşılan bir çalışmanın sonuçlarını içermektedir. Veri seti, uyku bozuklukları ve diğer sağlık parametreleri arasındaki ilişkileri incelemek amacıyla kullanılabilir. Kısaca özetleyecek olursak;

Toplam Gözlemler (Satırlar):
374 (Başlık satırı hariç),
Değişkenler (Sütunlar):
14,
Cinsiyet Dağılımı:
Erkek: 189
Kadın: 185

https://www.kaggle.com/datasets/vincentchege/sleep-and-lifestyle/data



# Grafikler

## Grafiklerin Çizdirilmesinde Yardımcı Paketlerin Yüklenmesi Ve Çalıştırılması

```

install.packages("readxl") #excell dosyasını okumak için
installed.packages("ggplot2") #ggplot görselleştirme araçlarını kullanmak için
install.packages("dplyr") #veri manipülasyonları için
install.packages("gridExtra") #grafik düzenleme paketi
install.packages("MetBrewer") #farklı renk paletleri sağlar
install.packages("tidyr") #veri çerçevelerindeki verilerin şeklini değiştirmek, düzenlemek ve temizlemek için 
library(readxl)
library(ggplot2)
library(dplyr)
library(gridExtra)
library(MetBrewer)
library(tidyr)

```


## Grafik-1 Cinsiyete Göre Uyku Bozukluğu Dağılımı

```

data <- read.csv("/cloud/project/Sleep Health and Lifestyle.csv")

colnames(data) #data verisi içerisindeki sütunların başlıklarını verir

# Cinsiyet ve uyku bozukluğu sütunlarını içeren bir alt veri kümesi oluşturma
sleep_disorder_by_gender <- data[, c("Gender", "Sleep.Disorder")]

# Renk paletini tanımlama
blue_palette <- colorRampPalette(c("#C7E9C0", "#4A90E2"))(3)  # 3 renk için palet oluştur


# Çubuk Grafiği
ggplot(sleep_disorder_by_gender, aes(x = Gender , fill = Sleep.Disorder)) +
  geom_bar(position = "dodge", width = 0.7, color = "white") +
  scale_fill_manual(values = blue_palette,
  labels = c("Uykusuzluk", "Hiçbiri", "Uyku Apnesi")) +
  labs(title = "Cinsiyete Göre Uyku Bozukluğu Dağılımı", x = "Cinsiyet", y = "Kişi Sayısı", fill = "Uyku Bozukluğu") +
  scale_x_discrete(labels = c("Kadın", "Erkek"))+
  theme_bw() +
  theme(
    plot.title = element_text(size = 16, hjust = 0.5),
    axis.title = element_text(size = 14,),
    axis.text = element_text(size = 12),
    legend.title = element_text(size = 14,),
    legend.text = element_text(size = 12))+
    ylim(0,150)

```
![Cinsiyet](https://github.com/Gozsever/Uyku_Yasam_Tarz-/blob/main/G%C3%B6rseller/Cinsiyet.png)

  Görsel analize göre, grafikte her iki cinsiyet için uyku bozuklukları kategorilerine göre kişi sayıları gösterilmektedir. Her iki cinsiyette de "Hiçbiri" kategorisinin diğer uyku 
  bozukluklarına göre daha yüksek olduğu görülebilir. Bu çoğu kişinin uyku bozukluğu yaşamadığını göstermektedir. Erkeklerde uyku bozukluğu "Hiçbiri" kategorisine baktığımızda kadınlara 
  göre daha fazla olduğunu görmekteyiz. Kadınlarda uyku apnesinin görülme oranı erkeklere göre daha yüksektir.

## Grafik-2 Meslek Kategorisine Göre Uykusuzluk (Insomnia) Dağılımı

```

# Meslek gruplaması yap
data <- data %>%
  mutate(Occupation_Grouped = case_when(
    grepl("Software Engineer", Occupation) ~ "Yazılım Mühendisi",
    grepl("Engineer", Occupation) ~ "Mühendis",
    grepl("Lawyer", Occupation) ~ "Avukat",
    grepl("Accountant", Occupation) ~ "Muhasebeci",
    grepl("Doctor", Occupation) ~ "Doktor",
    grepl("Salesperson", Occupation) ~ "Satış Personeli",
    grepl("Teacher", Occupation) ~ "Öğretmen",
    grepl("Nurse", Occupation) ~ "Hemşire",
    TRUE ~ Occupation  # "Diğer" kategorisinde olmayan meslekleri direkt olarak kullan
  )) %>%
  na.omit() 

# Meslek sıralaması
sorted_data <- data %>%
  group_by(Occupation_Grouped) %>%
  mutate(Count = n()) %>%
  arrange(Count, .by_group = TRUE)

# Renk paleti
blue_palette <- colorRampPalette(c("#C7E9C0", "#4A90E2"))(3)


# "insomnia" olanları filtrele
insomnia_data <- sorted_data %>%
  filter(Sleep.Disorder == "Insomnia")

# Görseli çizdirelim
ggplot(insomnia_data, aes(x = Count, y = reorder(Occupation_Grouped, Count), fill = as.factor(Sleep.Disorder))) +
  geom_bar(stat = "identity", position = "dodge") +
  scale_fill_manual(values = blue_palette,
  labels = c("Uykusuzluk"))+
  labs(
    title = "Meslek Kategorisine Göre Uykusuzluk (Insomnia) Dağılımı",
    y = "Meslek",
    x = "Gözlem Sayısı",
    fill = "Uyku Durumu"
  ) +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 0, hjust = 0.5))+  # Meslek isimlerini çevirerek düzenleme
  xlim(0, 90)

```
![Meslek ve Uykusuzluk](https://github.com/Gozsever/Uyku_Yasam_Tarz-/blob/main/G%C3%B6rseller/Meslek%20ve%20Uykusuzluk.png)

  Görsele baktığımızda, farklı meslek gruplarındaki uykusuzluk vakaları sayısal olarak gösterilmektedir. Hemşire ve doktor meslek gruplarının diğerlerine göre daha yüksek uykusuzluğa 
  sahip olduğu görülmektedir. Buna göre bu mesleklerin stresli veya yoğun çalışma koşullarına sahip olduğunu söyleyebiliriz. Ek olarak, yazılım mühendisliği gibi meslek gruplarında 
  uykusuzluk vakalarının daha düşük olduğu gözlemlenmektedir. Yazılım mühendislerinin daha düzenli çalışma saatlerine ve daha az stresli bir iş ortamına sahip olduğunu düşünebiliriz.
  
## Grafik-3 Yaşa Göre Stres Düzeyi Ve Uyku Kalitesi

```
# Stres düzeyi ve uyku kalitesi için verileri hazırlayalım
data_long <- data %>%
  select(Age, Stress.Level, Quality.of.Sleep) %>%
  gather(key = "Variable", value = "Value", -Age)

# Stres düzeyi ve uyku kalitesi için faktör seviyelerini ayarlama
data_long$Variable <- factor(data_long$Variable, levels = c("Stress.Level", "Quality.of.Sleep"),
                             labels = c("Stres Düzeyi", "Uyku Kalitesi"))

# Renk paletini oluşturduk
blue_palette <- c("#CAF0F8", "#90E0EF", "#00B4D8", "#0096C7", "#0077B6", "#023E8A", "#03045E")

# Yaş ortalamasını hesaplayalım
age_mean <- mean(data$Age, na.rm = TRUE)

# Birleştirilmiş violin grafiği oluşturma
ggplot(data_long, aes(x = as.factor(Value), y = Age, fill = as.factor(Value))) +
  geom_violin(trim = FALSE) +
  geom_boxplot(width = 0.2, fill = "white", color = "black") +
  scale_fill_manual(values = blue_palette) +
  facet_wrap(~ Variable, scales = "free_x") + 
  geom_hline(yintercept = age_mean, linetype = "solid", color = "red", linewidth = 0.5) +
  labs(
    title = "Yaşa Göre Stres Düzeyi ve Uyku Kalitesi",
    x = "Düzeyler",
    y = "Yaş",
    fill = "Düzeyler"
  ) +
  theme_minimal()

```
![Yaşa Göre Stres Ve Uyku](https://github.com/Gozsever/Uyku_Yasam_Tarz-/blob/main/G%C3%B6rseller/Ya%C5%9Fa%20G%C3%B6re%20Stres%20ve%20Uyku.png)

Görsel, stres düzeyi ve uyku kalitesine göre yaş dağılımlarını bizlere sunmaktadır. 

## Grafik-4 Uyku Bozukluklarının Uyku Kalitesine, Uyku Süresine, Stres Düzeyine Ve Fiziksel Aktiviteye Etkisi

```
 # Uykusuzluk ve Uyku Apnesi olan verileri filtrele
 filtered_data <- data %>% filter(Sleep.Disorder %in% c("Insomnia", "Sleep Apnea"))
 
 g3 <- ggplot(filtered_data, aes(x = Quality.of.Sleep, fill = Sleep.Disorder )) +
   geom_density(alpha = 0.5) +
   labs(x = "Uyku Kalitesi ",
        y = "Yoğunluk",
        title = "Uyku Bozukluklarının Uyku Kalitesine Etkisi",
        subtitle = "Kernel Yoğunluk Tahmini",
        fill= "Uyku Bozukluğu")+
   scale_fill_manual(values= met.brewer("Hokusai3",2),
                     labels = c("Uykusuzluk", "Uyku Apnesi")) +
   
   theme_bw() 
 
 
  g4 <-  ggplot(filtered_data, aes(x = Sleep.Duration, fill = Sleep.Disorder )) +
   geom_density(alpha = 0.5) +
   labs(x = "Uyku Süresi ",
        y = "Yoğunluk",
        title = "Uyku Bozukluklarının Uyku Süresine Etkisi",
        subtitle = "Kernel Yoğunluk Tahmini",
        fill= "Uyku Bozukluğu")+
   scale_fill_manual(values= met.brewer("Hokusai3",2),
                     labels = c("Uykusuzluk", "Uyku Apnesi")) +
   
   theme_bw() 
   
 g5 <-  ggplot(filtered_data, aes(x = Stress.Level, fill = Sleep.Disorder )) +
     geom_density(alpha = 0.5) +
     labs(x = "Stres Düzeyi ",
          y = "Yoğunluk",
          title = "Uyku Bozukluklarının Stres Düzeyine Etkisi",
          subtitle = "Kernel Yoğunluk Tahmini",
          fill= "Uyku Bozukluğu")+
     scale_fill_manual(values= met.brewer("Hokusai3",2),
                       labels = c("Uykusuzluk", "Uyku Apnesi")) +
     
     theme_bw() 
   
g6 <-   ggplot(filtered_data, aes(x = Physical.Activity.Level, fill = Sleep.Disorder )) +
     geom_density(alpha = 0.5) +
     labs(x = "Fiziksel Aktivite Düzeyi ",
          y = "Yoğunluk",
          title = "Uyku Bozukluklarının Fiziksel Aktiviteye Etkisi",
          subtitle = "Kernel Yoğunluk Tahmini",
          fill= "Uyku Bozukluğu")+
     scale_fill_manual(values= met.brewer("Hokusai3",2),
                       labels = c("Uykusuzluk", "Uyku Apnesi")) +
     
     theme_bw() 
   
grid.arrange(g3, g4, g5, g6)

```
![Uyku Bozukluğu](https://github.com/Gozsever/Uyku_Yasam_Tarz-/blob/main/G%C3%B6rseller/Uyku%20Bozuklu%C4%9Fu.png)

YORUM

# Sonuç 
  


