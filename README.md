# hse_hw3_chromhmm

В данной работе мы должны провести кластеризацию генома человека hg19 на основе данных экспериментов CHIPSeq для набора гистоновых меток. Клеточная линия была взята такая же как и в прошлом задании - DND41.
Код выполнения работы лежит в коллабе - https://colab.research.google.com/drive/1OSQdkssUITPP3wPNLl6CGURyKppGjEbw?usp=sharing

На входе у нас 12 файлов с размеченными на hg19 пиками гистоновых меток. Взял 12 (больше нет) в надежде получить лучший результат кластеризации так как при 10 вышло не очень.
Список файлов и меток:

H2az	DND41_H2az.bam

H3k27ac	DND41_H3k27ac.bam

H3k27me3	DND41_H3k27me3.bam

H3k36me3	DND41_H3k36me3.bam

H3k04me1	DND41_H3k04me1.bam

H3k04me2	DND41_H3k04me2.bam

H3k04me3	DND41_H3k04me3.bam

H3k79me2	DND41_H3k79me2.bam

H3k09ac	DND41_H3k09ac.bam

H3k09me3	DND41_H3k09me3.bam

Контроль для всех экспериментов это файл DND41_Control.bam

Результат работы chromhmm для 10 состояний:

![image](https://github.com/JustKeonix/hse_hw3_chromhmm/assets/24775932/7c4e7612-16f8-467c-98de-19dd21d5170c)
![image](https://github.com/JustKeonix/hse_hw3_chromhmm/assets/24775932/a828a770-face-4c50-9696-fdc2beec808f)

Из графиков сразу видно что кластер 1 это insulator так как находится полностью на пиках CTCF. 

Второй кластер это скорее всего polycomb repression так как по большей части находится на H3K27me3.

Кластер 4 похож на heterochromatin из-за H3K9me3.

В кластере 5 наиболее выражена метка H3K36me3. Если посмотреть на кластеризацию Encode на 15 состояний, то видно что они в таком случае определили это как transcriptional elongation
![image](https://github.com/JustKeonix/hse_hw3_chromhmm/assets/24775932/fa1b4a01-50af-4ebc-8e00-0f304fa1bd55)

6 кластер скорее всего это эказоны генов так как в нём много H3K36me3

С 7 клстером не уверен, пусть будет transcriptional transition

Кластер 8 является энхансером из-за H3K4me1

9 кластер скорее всего weak enchancer.

Промоутером является кластер 10 так как в нем больше всего H3K4me3 и он находится на CpG островке.

Рассчитаем дополнительные метрики, может станет понятней.

| Cluster | Distinctive Chromatin Modifications | Cluster Size (bp) | Average Length (bp) | Genomic Coverage (%) | Predicted Biological Function |
|---------|------------------------------------|-------------------|---------------------|----------------------|-------------------------------|
| 1       | CTCF                               | 20,066,800        | 536                 | 0.669                | Insulator                    |
| 2       | H3K27me3                           | 442,348,800       | 10,468              | 14.745               | Polycomb-repressed regions    |
| 3       | -                                  | 2,029,129,000     | 18,717              | 67.638               | Unknown1                      |
| 4       | H3K27me3                           | 49,520,000        | 1,811               | 1.651                | Heterochromatin               |
| 5       | H3K36me3                           | 142,308,400       | 3,794               | 4.744                | Transcriptional elongation    |
| 6       | H3K79me2, H3K4me2                  | 197,042,800       | 2,978               | 6.568                | Unknown2                      |
| 7       | H3K79me2, H4K20me1                 | 99,730,600        | 3,130               | 3.324                | Transcriptional transition    |
| 8       | H3K4me1, H3K27ac, H3K4me3          | 28,187,600        | 1,531               | 0.940                | Strong enhancer |
| 9       | H3K4me1, H3K27ac                   | 54,617,600        | 1,011               | 1.821                | Weak enhancer     |
| 10      | H3K4me2, H3K4me3, H3K27ac, H3K9ac  | 32,740,000        | 1,520               | 1.091                | Promoter       |

Кластер 3 не имеет в себе никаких выроженых меток, скорее всего из-за малого числа меток в эксперементе алгоритм кластеризации отнёс всё что не смог распознать в этот кластер. Это подтверждается черезмерно большим процентом занимемого генома данным кластером

В файле DND41_10_dense.bed были замененты номера кластеров на предсказанные названия. Резальтат завписан в новый файл DND41_10_dense_named.bed. Финальный результат в геномном браузере:

![image](https://github.com/JustKeonix/hse_hw3_chromhmm/assets/24775932/fd9dea2a-fb97-4717-8481-2dd8b2dbe7c9)

# Вывод
В ходе выполнения домашней работы мы кластеризовали геном на участки используя эпигенетические фактороы (гистоновые метки). У найденных кластеров была идентифицирована биологическая функция
