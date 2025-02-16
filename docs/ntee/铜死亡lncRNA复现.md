Prognostic signature construction and immunotherapy response analysis for Uterine Corpus Endometrial Carcinoma based on cuproptosis-related lncRNAs
## 数据来源
TCGA-UCEC
https://xenabrowser.net/datapages/?cohort=GDC%20TCGA%20Endometrioid%20Cancer%20(UCEC)&removeHub=https%3A%2F%2Fxena.treehouse.gi.ucsc.edu%3A443
- 表达量（包含lnc）
	- 原60660\*585
	- 整理后59360\*488
- 临床信息（年龄、良恶性、stage）（没有TNM分期）
	- 488
- 生存信息（OS、OS.time）
	- 488

lncRNA集
	去重后16000多

基因-探针映射（需去除小数点）

GEO数据（R中可以下载）

铜死亡相关基因（来源于前期研究）（19）
~~~R
target_genes <- c("NFE2L2", "NLRP3", "ATP7B", "ATP7A", "SLC31A1", "FDX1", "LIAS", "LIPT1", "LIPT2", "DLD", "DLAT", "PDHA1", "PDHB", "MTF1", "GLS", "CDKN2A", "DBT", 
"GCSH", "DLST")
~~~
## 数据整理
我的课题也可以用这些数据，都是子宫内膜癌的
疑问：表达数据里有很多整数怎么回事，而且是和小数交错出现（比如下面第二行）
![[Pasted image 20241121151451.png]]
#### 基因表达矩阵
表达量矩阵、探针矩阵去除小数点
基因名称映射
（如有需要）log处理、归一化处理...
去重（方法众多）
合并数据 **（一定要先去重再融合，否则会多出很多莫名其妙的项）**
去除临床信息矩阵中未出现的样本

lncRNA映射（我没直接下载到名称，所以要转换一下）
去重
从表达量矩阵中提取对应数据
#### 临床信息整理
将生存信息合并到临床信息中，注意样本要对应
（可选）修改行名


## 铜死亡相关lncRNA筛选
“Based on the expression levels of lncRNAs and CRGs, Pearson correlation analysis was adopted to determine the potential CRLs (P <0.001, | Pearson R |> 0.4) and draw the Sankey diagram of the coexpression relationship.”

sankey图感觉没什么用懒得画了

Pearson correlation 过程很慢（因为要双层循环，每次内层要遍历16000多个lncRNA），建议打印中间值缓解等待焦虑
~~~R
[1] "NFE2L2"
[1] 0 4
[1] "NLRP3"
[1] 949   4
[1] "ATP7B"
[1] 1052    4
~~~
筛选结果：去重前13417个，去重后2968个（和exp的处理有关）

作者用Pearson correlation R>0.4筛选，这个P<0.001有点奇怪，我的结果在10E-24~10E-160量级，不知道为什么这么小
我又随机抽了结果中的两行单独计算，确实有这么小，估计是因为样本量太大了
~~~R
library(readxl)
dt1 <- read_excel("C:/Users/Enki/Desktop/pppp.xlsx")
dt1 <- as.data.frame(dt1)
rownames(dt1) <- dt1[, 1]
dt1 <- dt1[, -1]
dt1 <- as.matrix(dt1)
result <- cor.test(dt1["NFE2L2", ], dt1["FGD5-AS1", ], method = "pearson")
correlation <- result$estimate
p_value <- result$p.value
> correlation
[1] 0.8320935 
> p_value
[1] 2.263616e-151
~~~

作者没有提供中间数据，即筛选出来了哪些基因（这个对于复现很重要，文章太简略了）、哪些基因之间有相关性...

## lasso-cox 分析
#### 单因素cox分析
（合并后的）临床矩阵随机分为两组，一组用于构建模型另一组用于验证
	关于1：1拆分有待商榷，个人认为可以4：1或其它

本人构建模型直接取了前300，因为随机所以必然无法得到一模一样结果。作者没有提供数据，发邮件询问也没有回复

临床矩阵的生存状态（OS）、生存时间（OS.time）信息
根据上一步筛选的基因提取表达量信息，转置并按照临床矩阵中的样本顺序排列，以待合并

单因素cox初筛：遍历筛选到的基因，动态地将基因合并到临床矩阵末尾，进行cox计算，当p < 0.05 时保留，运算后及时删去基因列
	正好筛出了1000个
#### Lasso-cox回归
根据初筛的基因做lasso-cox回归，lambda.min=0.04344
###### U型图、轨迹图
![[Pasted image 20241121191951.png|400]]
注：横坐标中的log其实是ln
###### 基因再筛
去除系数为0的基因，筛选出15个
左边是我的，右边是人家的
![[Pasted image 20241121192207.png|300]]
令人欣慰的是居然有重合的：AC078860.3  LINC01224
文章中的29个有10个在我单因素cox初筛的1000个里
~~~R
# 重合的10个
c("Z99572.1", "NNT-AS1", "HMGN3-AS1", "AL035530.2", "AC244517.1", "AL031778.1", "AC078860.3", "BX322234.1", "CDKN2A-DT", "LINC01224")
# 我的15个
c("PRDX6-AS1", "AL603832.1", "TNFRSF10A-AS1", "AC078860.3", "AP003096.1", "AP001160.1", "AC104758.1", "AC055764.2", "AL035603.1", "AC004884.2", "LINC01929", "AC107057.1", "AC092969.1", "AC020934.1", "LINC01224")
# 人家的29个
c("RAB11B-AS1", "HMGN3-AS1", "AC244517.7", "AF131215.5", "AC007552.2", "Z69733.1", "AL031778.1", "AL035530.2", "AC083799.1", "AC118344.1", "AC244517.1", "MIR924HG", "LINC00954", "AC092614.1", "AL161663.2", "AP000787.1", "AL158212.3", "LINC02035", "AC026202.2", "BX322234.1", "AC103563.2", "LINC01224", "AC104794.5", "NNT-AS1", "AC078860.3", "Z99572.1", "AC006504.5", "AL450384.1", "CDKN2A-DT")
~~~
#### 多因素cox分析
将筛到的15个基因从exp中提取表达信息，一个基因为一列按照临床样品的顺序添加，以便于cox分析
## 附代码
#### exp, clin数据整理
~~~R
setwd("C:/Users/Enki/Desktop/UCEC")
library(tidyverse)
library(dplyr)
library(stringr)
library(limma)
library(sva)
library(clusterProfiler)
library(tinyarray)
library(data.table)
# check.names 一定要f，如果你想省事的话
exp <- read.table("TCGA-UCEC.star_counts.tsv", header=T,sep ="\t",check.names=F) 
exp[[1]] <- sapply(exp[[1]], function(x) strsplit(x, "\\.")[[1]][1]) 

# ID转化
map <- read.table("gencode.v36.annotation.gtf.gene.probemap", header=T,sep ="\t",check.names=F)
map[[1]] <- sapply(map[[1]], function(x) strsplit(x, "\\.")[[1]][1]) 
map<- map[!duplicated(map[[1]]), ]
# 取整后ID和gene列均有重复，原60660列，若直接融合会变为60748列（(60660-60616)*2+60660=60784）
exp <- exp[!duplicated(exp[[1]]), ]
exp <- merge(exp, map, by.x = "Ensembl_ID", by.y = "id", all = FALSE )
# 将 gene列移到前面并删除ID列
gene_index <- which(names(exp) == "gene")
exp <- exp[, c(1, gene_index, setdiff(seq_along(exp), c(1, gene_index)))]
exp <- exp[, -1]
# gene_counts <- table(exp[[1]])

# 将结果转换为数据框，并筛选出出现次数大于 1 的基因
duplicate_genes <- as.data.frame(gene_counts[gene_counts > 1])
colnames(duplicate_genes) <- c("Gene", "Count")

sum <- sum(duplicate_genes$Count, na.rm = TRUE)

# exp %>% aggregate(.~gene, FUN=mean) # 这个巨慢，没开CUDA慎用
exp1 <- exp[!duplicated(exp[,1]),] # 所以我偷懒了

data <- read.table("TCGA-UCEC.clinical.tsv", header=T,sep ="\t") 
data1 <- data[,c("sample","tissue_type.samples","figo_stage.diagnoses","age_at_index.demographic")]
#MX cM0 (i+)
os <- read.table("TCGA-UCEC.survival.tsv", header=T,sep ="\t",check.names=F) 
os <- os[,-4]

clin <- merge(os, data1, by.x = 1, by.y = 1) 
clin1 <- clin[clin$sample %in% colnames(exp), ] 
# 一行代码整理分期，十分好用
clin1$figo_stage.diagnoses <- gsub("[ABC123]", "", clin1$figo_stage.diagnoses, ignore.case = TRUE)
library(openxlsx)
write.xlsx(clin1, "clincal.xlsx")
~~~
#### Pearson correlation 铜死亡相关lncRNA筛选
~~~R
setwd("C:/Users/Enki/Desktop/UCEC")
library(survival)
library(dplyr)
library(tidyverse)
library(readxl)
library(networkD3)

exp <- read_excel("C:/Users/Enki/Desktop/UCEC/exp.xlsx")
exp <- as.data.frame(exp)
duplicated_rows <- duplicated(exp[, 1]) | duplicated(exp[, 1], fromLast = TRUE)
exp <- exp[!duplicated_rows, ]
rownames(exp) <- exp[, 1]
exp <- exp[, -1]

# exp <- log2(exp + 1) 

#exp <- t(scale(t(exp)))
#normalize <- function(x) (x - min(x)) / (max(x) - min(x))
#exp <- apply(exp, 1, normalize)

# 铜死亡相关基因
target_genes <- c("NFE2L2", "NLRP3", "ATP7B", "ATP7A", "SLC31A1", "FDX1", "LIAS", "LIPT1", "LIPT2", "DLD", "DLAT", "PDHA1", "PDHB", "MTF1", "GLS", "CDKN2A", "DBT", "GCSH", "DLST")


# 获取16000多lncRNA
library(biomaRt)
# 连接到 Ensembl 数据库
ensembl <- useMart("ensembl", dataset = "hsapiens_gene_ensembl")
# 获取 lncRNA 基因
lncRNA_genes <- getBM(
  attributes = c("ensembl_gene_id"),
  filters = "biotype",
  values = "lncRNA",
  mart = ensembl
)
# 去重
lncRNA_genes <- distinct(lncRNA_genes)
map <- read.table("gencode.v36.annotation.gtf.gene.probemap", header=T,sep ="\t",check.names=F)
map[[1]] <- sapply(map[[1]], function(x) strsplit(x, "\\.")[[1]][1]) 
map <- map[!duplicated(map[[1]]), ]

lncRNA_genes <- merge(lncRNA_genes, map, by.x = "ensembl_gene_id", by.y = "id", all = FALSE )
lncRNA_genes <- distinct(lncRNA_genes)
				   
# 清空变量是个好习惯
cor_results <- data.frame(
  Gene1 = character(),
  Gene2 = character(),
  Correlation = numeric(),
  P.value = numeric(),
  stringsAsFactors = FALSE
)
other_genes <- ""
pop <- ""

for (pop in target_genes) {
  print(pop)
  print(dim(cor_results))
  for (other_gene in lncRNA_genes$gene) {
    if (pop != other_gene) {
      # 获取 lncRNA 的表达数据
      test_data1 <- as.numeric(exp[pop, ])
      test_data2 <- as.numeric(exp[other_gene, ])
      
      # 去除缺失值
      valid_idx <- complete.cases(test_data1, test_data2)
      test_data1 <- test_data1[valid_idx]
      test_data2 <- test_data2[valid_idx]
      
      # 跳过表达值全为0的 lncRNA 基因
      if (all(test_data2 == 0, na.rm = TRUE)) {
        next
      }
      
      # 只有当数据量大于 1 时才进行相关性计算
      if (length(test_data1) > 1 && length(test_data2) > 1) {
        test <- cor.test(test_data1, test_data2, method = "pearson")
        
        # 检查相关性估计值和 p 值是否为有效数值
        if (is.finite(test$estimate) && is.finite(test$p.value)) {
          # 筛选符合条件的基因对
          if (abs(test$estimate) > 0.4 && test$p.value < 0.001) {
            cor_results <- rbind(cor_results, data.frame(
              Gene1 = pop,
              Gene2 = other_gene,
              Correlation = test$estimate,
              P.value = test$p.value
            ))
          }
        } else {
          next  # 如果相关性估计或 p 值无效，跳过
        }
      } else {
        next  # 如果有效数据点太少，跳过该次循环
      }
    }
  }
}

foo <- cor_results$Gene2
foo<-unique(foo)
write.csv(foo, "foo.csv", row.names = FALSE)
~~~
#### lasso-cox分析（多因素cox还没做）
~~~R
library(survival)
library(glmnet)
library(survminer)

foo <- read.csv("foo.csv")
exp <- read_excel("C:/Users/Enki/Desktop/UCEC/exp.xlsx")
exp <- as.data.frame(exp)
duplicated_rows <- duplicated(exp[, 1]) | duplicated(exp[, 1], fromLast = TRUE)
exp <- exp[!duplicated_rows, ]
rownames(exp) <- exp[, 1]
exp <- exp[, -1]
clin <- read_excel("C:/Users/Enki/Desktop/UCEC/clincal.xlsx")
clin <- as.data.frame(clin)
rownames(clin) <- clin$sample
clin <- clin[, -1]

clin1 <- head(clin, 300)
expm1 <- head(exp_matrix,300)

# 对每个基因进行单变量 Cox 回归分析
# 初始化一个空的结果数据框
univariate_results <- data.frame()
# 初始化结果存储数据框
univariate_results <- data.frame(Gene = character(), Coef = numeric(), P_value = numeric(), stringsAsFactors = FALSE)

# 遍历foo$x中的目标基因
for (ge in foo$x) {
    gene_data <- exp[ge, rownames(clin1)] 
    gene_data <- as.vector(gene_data) # 单行的转置
    gene_data <- as.numeric(gene_data)
    # 将基因数据作为clin数据框的一列
    clin1$gene <- gene_data
    
    cox_model <- coxph(Surv(OS.time, OS) ~ gene, data = clin1)
    cox_summary <- summary(cox_model)
    
    # 提取相关系数和 p 值
    coef <- cox_summary$coefficients[1, 1]  # 提取第一个系数
    p_value <- cox_summary$coefficients[1, 5]  # 提取p值，通常是第五列
    
    # 只有当 p < 0.05 时才筛选
    if (p_value < 0.05) {
      univariate_results <- rbind(univariate_results, data.frame(Gene = ge, Coef = coef, P_value = p_value))
    }
    clin1$gene <- NULL
  }

#write.csv(univariate_results, "univariate_results.csv", row.names = FALSE)

# 使用 Lasso-Cox 回归进行特征选择
# 保留初筛的基因，添加表达量信息
exp_matrix <- exp
exp_matrix <- exp_matrix[, rownames(clin1), drop = FALSE]
exp_matrix <- exp_matrix[univariate_results$Gene,]
exp_matrix <- exp_matrix[, match(rownames(clin1), colnames(exp_matrix)), drop = FALSE]
exp_matrix <- t(exp_matrix)

surv_data <- Surv(time = clin1$OS.time, event = clin1$OS)

# 数据标准化
expm1_scaled <- scale(exp_matrix)  # 对表达矩阵进行标准化

# 使用glmnet绘制轨迹图
lasso_model_new <- glmnet(
  x = expm1_scaled,           
  y = surv_data,               
  alpha = 1,                  # L1正则化
  family = "cox",             # Cox比例风险回归
  type.measure = "deviance",  # 使用偏差（Deviance）作为评估标准
  nfolds = 10,                # 10flod交叉验证
)
plot(lasso_model_new, xvar = "lambda", xlim = c(-5, -2.3), ylim = c(-1 , 1))

# 使用cv.glmnet绘制U型图
lasso_model_new <- cv.glmnet(
  x = expm1_scaled,          
  y = surv_data,               
  alpha = 1,                   
  family = "cox",              
  type.measure = "deviance",   
  nfolds = 10,                
)
plot(lasso_model_new, xlim = c(-5, -2.2), ylim = c(9, 15)) 


# 获取最优lambda对应的系数
lambda_best <- lasso_model_new$lambda.min
coefficients <- coef(lasso_model_new, s = lambda_best)

# 转换为数据框格式并去除系数为零的基因
coefficients_matrix <- as.matrix(coefficients)
coefficients_df <- data.frame(Gene = rownames(coefficients_matrix), Coefficient = coefficients_matrix[, 1])
coefficients_df <- coefficients_df[coefficients_df$Coefficient != 0, ]  # 去除系数为零的基因

# 原文章数据验证
sql <- c("RAB11B-AS1", "HMGN3-AS1", "AC244517.7", "AF131215.5", "AC007552.2", "Z69733.1", "AL031778.1", "AL035530.2", "AC083799.1", "AC118344.1", "AC244517.1", "MIR924HG", "LINC00954", "AC092614.1", "AL161663.2", "AP000787.1", "AL158212.3", "LINC02035", "AC026202.2", "BX322234.1", "AC103563.2", "LINC01224", "AC104794.5", "NNT-AS1", "AC078860.3", "Z99572.1", "AC006504.5", "AL450384.1", "CDKN2A-DT")
filtered_data <- subset(univariate_results,Gene%in% sql)
~~~