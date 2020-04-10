### OPE的安全性定义的总结
保序加密(OPE)这一概念在最初由Agrawal等人提出后，出现了许多方案，许多给出OPE方案的文献中都给出了针对所提出方案的安全性分析，比如从数学的角度证明安全，但是其中很少有正式的语义安全定义<sup>[1]</sup>，也就是没有指出可能会泄露什么信息，达到哪种层次的安全性。下表总结了近些年文献中提出的OPE的几种正式的安全性定义，可能还有待完善，需要在后续的研究中继续增添修改：
|  相关文献 | 安全性定义  |提出年份|
|  :----:  | :----:  |:----:|
| 1  | IND-OCPA, POPF-CCA |2009|
| 2  | Window One-Wayness |2011|
| 3  | ![](http://latex.codecogs.com/gif.latex?\\(\chi, \theta, q))-Indistinguishability |2014|
| 4  | IND-FAOCPA |2015|
| 5 | IND-CPA-DS |2019|
1. Alexandra Boldyreva, Nathan Chenette, Younho Lee, Adam O'Neill: Order-Preserving Symmetric Encryption. EUROCRYPT 2009: 224-241
2. Alexandra Boldyreva, Nathan Chenette, Adam O'Neill: Order-Preserving Encryption Revisited: Improved Security Analysis and Alternative Solutions. CRYPTO 2011: 578-595
3. Isamu Teranishi, Moti Yung, Tal Malkin: Order-Preserving Encryption Secure Beyond One-Wayness. ASIACRYPT (2) 2014: 42-61
4. Florian Kerschbaum: Frequency-Hiding Order-Preserving Encryption. ACM Conference on Computer and Communications Security 2015: 656-667
5. 	Florian Kerschbaum, Anselme Tueno:An Efficiently Searchable Encrypted Data Structure for Range Queries. ESORICS (2) 2019: 344-364

#### IND-OCPA
在Agrawal等人首次提出OPE这个概念后，一直没有出现对于OPE的正式安全性定义，IND-OCPA安全性定义是由Boldyreva等人首次提出。IND-OCPA(Indistinguishability Under Ordered Chosen-Plaintext Attack)也就是有序选择明文攻击下的不可区分性。IND-OCPA定义可以看作是语义安全性(Semantic Security)的延伸。这个安全性定义的含义是：对于消息序列加密后的密文，除了消息的顺序之外，不应泄露任何其他信息。随后Boldyreva等人证明了，即使在密文空间大小是明文空间大小的指数倍的情况下，也无法构造有效的OPE方案，使之可以满足IND-OCPA安全。此方案的安全性被认为是最强的，直到2013前Popa等人<sup>[2]</sup>通过使用一个服务器端可变的数据结构来存储数据实现前，没有人实现此安全性。然而2015年Naveed等人<sup>[3]</sup>提出的Inference Attacks方法指出，即使是满足IND-OCPA定义的方案，仍然可以被攻击（频率分析攻击）。

#### POPF-CCA
POPF-CCA安全性是和IND-OCPA定义都是由相关文献1提出，因为作者在提出IND-OCPA定义时说明此定义很难实现，所以给出了一个安全性更弱的POPF-CCA安全性定义，并且给出了一个符合此安全性定义的方案。此安全性POPF-CCA(Pseudorandom Order-Preserving Function against Chosen-Ciphertext Attack)指的是将OPE方案的加密函数与保留顺序的随机映射函数进行了比较，也就是说，OPE方案的加密算法表现出来的结果类似于真正的随机化的顺序保存函数。但是，POPF-CCA安全定义未精确指出实现该定义的OPE方案泄漏的信息有什么。实际上，实现这种安全性的方案甚至不能满足单个加密密文的语义安全性，所以此定义安全性和实用性都很差。

#### Window One-Wayness
Window One-Wayness是Boldyreva等人在提出POPF-CCA安全性后的补充。严格来说不算是安全性定义，而是衡量一个方案对于顺序的泄露量的定义。Window One-Wayness指的是，给对手一个有序明文序列![](http://latex.codecogs.com/gif.latex?\\m_1 \leq m_2 \leq m_3 \leq \cdots \leq m_n)的对应的有序密文序列![](http://latex.codecogs.com/gif.latex?\\c_1 \leq c_2 \leq c_3 \leq \cdots \leq c_n)，随后从明文空间中以均匀概率选择一![](http://latex.codecogs.com/gif.latex?\\m)，将![](http://latex.codecogs.com/gif.latex?\\m)的对应密文![](http://latex.codecogs.com/gif.latex?\\c)给对手，对手需要给出一个估计的明文值![](http://latex.codecogs.com/gif.latex?\\\hat{m})和一个范围![](http://latex.codecogs.com/gif.latex?\\r)，使得![](http://latex.codecogs.com/gif.latex?\\c)的明文![](http://latex.codecogs.com/gif.latex?\\m)在所给的范围![](http://latex.codecogs.com/gif.latex?\\(\hat{m}-r,\hat{m}+r))内的概率大于1/2。此安全性定义主要在于分析密文顺序的泄露情况。如果方案能不泄露顺序，那么对手给出正确范围的几率和在范围值![](http://latex.codecogs.com/gif.latex?\\r)内随机猜测的概率是相同的。

#### ![](http://latex.codecogs.com/gif.latex?\\(\chi, \theta, q))-Indistinguishability
Teranishi等人在文献3中指出IND-OCPA的安全性较难满足，并且被证明即使在密文空间大小是明文空间大小的指数倍的情况下也没有有效的方案，而POPF-CCA的安全性又太低，Boldyreva等人也证明了自己的一个满足POPF-CCA安全的方案会泄露每个密文一半比特数（低位的比特）。所以Teranishi等人提出了一个折衷的方案。并且指出如果满足![](http://latex.codecogs.com/gif.latex?\\(\chi, \theta, q))-Indistinguishability，就不会泄露密文的一半比特数。这个安全性定义基于的游戏是，由![](http://latex.codecogs.com/gif.latex?\\Mg)(Message Generator)生成![](http://latex.codecogs.com/gif.latex?\\(m_0^*, m_1^*, info))给对手，并且选择![](http://latex.codecogs.com/gif.latex?\\q)个明文加密成密文给对手，随后![](http://latex.codecogs.com/gif.latex?\\Mg)生成一个比特![](http://latex.codecogs.com/gif.latex?\\b)，将![](http://latex.codecogs.com/gif.latex?\\m_b^*)加密发给对手。如果对手能从这些已有的信息推断，以大于1/2的概率正确给出![](http://latex.codecogs.com/gif.latex?\\b)的值，就说明对手赢得游戏。其中，参数![](http://latex.codecogs.com/gif.latex?\\info)是一个比特串，包含了![](http://latex.codecogs.com/gif.latex?\\(m_0^*, m_1^*))的一些信息。![](http://latex.codecogs.com/gif.latex?\\\chi)是指在随机选择![](http://latex.codecogs.com/gif.latex?\\q)个明文的时候消息的概率分布函数，![](http://latex.codecogs.com/gif.latex?\\\theta)指的是![](http://latex.codecogs.com/gif.latex?\\m_0^*, m_1^*)的值需要满足不等式![](http://latex.codecogs.com/gif.latex?\\|m_1^*-m_0^*| \leq \theta)。如果对手在获得这些信息的情况下仍无法以大于1/2的概率赢得游戏，就说明这个方案是满足![](http://latex.codecogs.com/gif.latex?\\(\chi, \theta, q))-Indistinguishability这个安全定义的。

#### IND-FAOCPA
此安全性定义是在IND-OCPA安全性的基础上，添加了不泄露频率信息而得来的，所以安全性比IND-OCPA强。作者发现满足IND-OCPA安全性的方案在大量密文的情况下，仍然可能被频率分析攻击，并且密文空间越大，越容易受到攻击，所以相应的提出了IND-FAOCPA(Indistinguishability Under Frequency-analyzing Ordered Chosen Plaintext Attack)安全性定义，即隐藏频率的有序选择明文攻击下的不可区分性。这个安全性定义的含义是：对消息序列的加密的密文除了对消息的顺序之外，不应透露任何其他信息，包括明文的频率特征。

#### IND-CPA-DS
这个安全性定义是Kerschbaum等人等人基于IND-OCPA，并且考虑了Naveed等人的频率分析攻击后最新提出来的一种OPE安全性定义。所以也是迄今安全性最高的安全性定义。IND-CPA-DS指的是Efficiently Searchable Encrypted Data Structure is Indistinguishable Under a Chosen-Plaintext Attack，也就是一个高效的可搜索加密的数据结构，在选择明文攻击下是无法区分的。作者通过实验定义这个安全性。在这个实验中对手给出一个正确的索引和明文对的概率，是对手选择的明文在两个数据结构中的总出现频率。如果对手的正确概率与这个频率的差小到可以忽略，说明这个数据结构是在选择明文攻击下不可区分的，也就是IND-CPA-DS安全的。总的来说，对手不能从加密的OPE数据结构中得到包括顺序，频率在内的任何信息。这个安全性定义非常强，目的就是防御Naveed等人<sup>[3]</sup>和Grubbs等人<sup>[4]</sup>提出的针对OPE和ORE方案的几种攻击。如果满足这个安全性定义，那么对手无法从存储数据的数据结构中获得任何顺序，频率，排列等信息。


#### 注释：
[1] Kerschbaum等人在两篇文献中都提到了这个问题。分别是相关文献4和相关文献5。

> 文献4：There is also a large number of other order-preserving encryption schemes [4, 19, 20, 24, 25, 26, 28, 38] which provide no formal, but rather ad-hoc security analysis, including the original proposal by Agrawal et al. 
> 文献5：There are many more order-preserving encryption schemes that have been proposed in the literature [1], [27], [31], [32], [39], [42], [44], [45], [46], [53], [62], [63], [64] which we do not discuss here, since they lack a formal security analysis.  

[2] Raluca A. Popa, Frank H. Li, Nickolai Zeldovich: An Ideal-Security Protocol for Order-Preserving Encoding. IEEE Symposium on Security and Privacy 2013: 463-477  

[3] Muhammad Naveed, Seny Kamara, Charles V. Wright: Inference Attacks on Property-Preserving Encrypted Databases. ACM Conference on Computer and Communications Security 2015: 644-655  

[4] Paul Grubbs, Kevin Sekniqi, Vincent Bindschaedler, Muhammad Naveed, Thomas Ristenpart: Leakage-Abuse Attacks against Order-Revealing Encryption. IEEE Symposium on Security and Privacy 2017: 655-672