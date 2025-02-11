*此条目用于记录压缩包加密及破解原理的研究过程。*

## 提出问题

现象级：常见的破解手段？为什么可以被破解？如何破解？

技术级：文件的压缩过程？期间的加密过程是什么？

思想级别：加密和解密算法的实现

基础科学：压缩加密的底层数学算法？



## 资料研究

### 常见破解手段

**1. 暴力破解（Brute Force Attack）**

**原理**：逐个尝试所有可能的字符组合，直到找到正确的密码。

**适用性**：当密码较短或简单时，暴力破解相对有效。如果密码较长且复杂，破解时间可能非常长。

**2. 字典攻击（Dictionary Attack）**

**原理**：使用常见的密码字典文件逐个尝试可能的密码。

**适用性**：对弱密码（常见密码）非常有效，攻击速度比暴力破解快。

### 为什么可以被破解

虽然现代加密算法（如 AES）在理论上非常安全，但加密的压缩包文件仍可能通过一些技术或手段被破解。破解方法通常是针对加密的实现或攻击密码本身的弱点，而不是直接攻击算法。

**暴力破解**是一种最简单、最直接的密码破解方法。它通过枚举所有可能的密码组合来尝试解密压缩包。尽管这在理论上可行，但它非常依赖密码的复杂度。

**暴力破解的挑战：**

​	•	**密码长度和复杂度**：如果密码非常长（如 10 个字符以上）且包含大写字母、数字、特殊符号等，暴力破解所需的时间将会呈指数增长。例如，使用 8 个字符的密码（包含字母、数字和符号）可能需要数年甚至更长的时间才能通过暴力破解解密。

​	•	**计算能力**：暴力破解的时间也与攻击者的计算资源相关。如果攻击者拥有强大的计算设备，如集群计算或 GPU 加速，他们可以更快地枚举密码组合。

​	•	**AES的安全性**：AES等现代加密算法对抗暴力破解的能力非常强，因为它们能够处理非常复杂的密钥，且密钥空间极大（AES-256 的密钥空间有 2^{256} 种可能性），使得暴力破解几乎不可行。

**为什么暴力破解还是可能成功？**

​	•	**弱密码**：如果用户设置了简单或常见的密码，如“12345”或“password”，那么暴力破解可以在短时间内通过尝试常见密码或简单组合快速破解。这就是为什么选择复杂、难以猜测的密码如此重要。

​	•	**字典攻击**：暴力破解的变种是**字典攻击**，它不依赖穷举所有可能的密码，而是先尝试使用常见的密码列表（如常见的单词、短语、生日等）。字典攻击比纯粹的暴力破解快得多，尤其是当用户使用了常见密码时。

### 防破解建议

**使用强密码**：密码长度至少8位，并包含数字、大小写字母、符号等。

**加密算法选择**：选择支持AES加密的压缩格式（如RAR5），因为这种算法难以通过普通手段破解。

**多因素认证**：对于敏感文件，最好不要仅依赖压缩包密码保护，可以将文件加密后再进行压缩。

**避免使用常见密码**：避免使用如“123456”、“password”等简单密码，降低被字典攻击破解的可能性。

### 无损压缩定义和过程

**无损压缩**

无损压缩意味着压缩前后的文件内容是完全一致的，信息不会丢失。常用于文本文件、程序、图像（如PNG）和其他对数据完整性要求很高的文件。常见的无损压缩算法有：

​	•	**Huffman编码**：通过使用较短的二进制编码替代出现频率较高的字符，减少文件体积。

​	•	**LZ（Lempel-Ziv）算法**：通过找到文件中重复的模式和结构，使用指针引用代替这些重复的数据来压缩文件。常见的应用包括ZIP、GZIP、7z等格式。

**举例**

假设有一个文本文件，其中包含 “hello hello hello”，LZ算法可能会将第二个和第三个“hello”替换为对第一个“hello”的引用，从而减少占用的空间。

**压缩步骤**

​	1.	**分析文件内容**：算法首先分析文件内容，寻找可以压缩的冗余信息（如重复的数据模式）。

​	2.	**应用压缩算法**：根据分析结果，使用适当的算法进行压缩，将冗余信息删除或替换为更简短的表示方式。

​	3.	**生成压缩文件**：输出一个新的、更小的文件（如ZIP文件）。



### 解压缩过程（以解压ZIP文件为例）

解压无损压缩文件的过程是将压缩时的所有信息恢复成原始状态。在无损压缩中，压缩过程不会丢失数据，因此解压后文件内容与原始文件完全一致。

以解压 **ZIP** 文件为例，详细说明解压的过程：

**1. 读取文件头**

每个 ZIP 文件都包含一个**文件头**，其中记录了压缩文件的信息，如文件名、压缩方法、文件大小、压缩后大小、校验值（CRC-32），以及其他一些元数据。在解压过程中，解压程序首先读取这个文件头，了解每个文件的基本信息。

**文件头内容**：

​	•	**文件签名**：标记为 ZIP 格式文件。

​	•	**版本信息**：记录创建 ZIP 文件时使用的压缩工具版本。

​	•	**压缩方法**：ZIP 文件中可以使用不同的压缩方法，如 store（无压缩）、deflate（最常见的压缩算法）。

​	•	**文件大小信息**：包含压缩前和压缩后的文件大小。

​	•	**CRC-32 校验值**：用于验证文件内容的完整性。

**2. 解压缩数据块**

ZIP 文件通过不同的压缩算法对每个文件的数据块进行压缩，例如常见的 **DEFLATE** 算法。解压过程中，解压程序读取压缩后的数据块，并通过与压缩时相反的操作恢复原始数据。

​	•	**DEFLATE 解压**：这是 ZIP 常用的压缩算法，结合了 **LZ77** 算法（通过查找重复的数据片段进行压缩）和 **Huffman 编码**（通过赋予常用字符更短的二进制编码减少文件体积）。解压程序首先找到重复数据的引用，然后将它们展开，并根据 Huffman 编码将字符映射到原始字符集，最终恢复出完整的文件数据。

**3. 重建文件结构**

解压程序按照文件头的信息（如文件名、路径等）在目标文件夹中重建原始文件的目录结构。ZIP 文件可以包含多个文件和文件夹，解压程序会将这些文件解压到指定的目录中，并重建它们的原始路径。

**4. 校验文件完整性**

ZIP 文件的每个文件都有一个 **CRC-32** 校验值，用于验证文件的完整性。在解压完成后，解压程序会计算解压出来的文件内容的 CRC-32 值，并与压缩时记录的值进行比对。如果两者一致，则说明文件解压成功且未被损坏。

**5. 输出解压文件**

当所有数据都解压完成并通过校验，解压程序将生成解压后的文件。在这个过程中，文件的内容、大小、时间戳等属性都与原始文件保持一致。

**ZIP 解压流程总结**

​	1.	**读取 ZIP 文件头**：提取文件元数据。

​	2.	**解压每个文件的压缩数据**：根据压缩算法将文件还原。

​	3.	**重建文件结构**：在目标文件夹中重建原始目录结构。

​	4.	**校验完整性**：使用 CRC-32 校验值确保文件未损坏。

​	5.	**生成解压文件**：完成文件的解压输出。

### 加密/解密过程

在 ZIP 文件压缩和解压的过程中，**添加密码并使用密码加密**的过程是在数据压缩之后、文件头生成之前进行的，具体步骤可以拆解为以下几个阶段：

**1. 数据压缩阶段（与无密码压缩相同）**

​	•	在压缩包加密之前，ZIP 工具会首先按照常规步骤对文件数据进行压缩（如使用 **DEFLATE** 算法）。

​	•	生成压缩的数据块后，这些数据将成为加密的对象。

**2. 加密阶段**

在压缩完成后，ZIP 工具会使用指定的密码对压缩后的数据进行加密。这一阶段发生在数据压缩之后，但在将数据写入最终 ZIP 文件之前。

​	•	**加密算法**：ZIP 文件的加密方式有多种，常见的包括传统的 ZIP 加密（较弱）和现代更安全的 **AES（Advanced Encryption Standard）加密**。许多现代 ZIP 工具支持 AES-256 加密，这种加密方式比传统 ZIP 加密要更强大和安全。

​	•	**加密过程**：当用户设置密码时，ZIP 工具会基于这个密码生成一个密钥，然后使用密钥对压缩数据块进行加密。数据块的内容在加密后变得不可读，只有使用正确的密码解密才能恢复出原始内容。

​	•	**加密文件头**：部分 ZIP 文件格式会对文件头中的一些敏感信息（如文件名、文件大小等）进行额外加密，以防止他人从未解密的文件头中获取过多的信息。

**3. 生成文件头和写入加密数据**

在数据加密完成后，ZIP 工具会生成 ZIP 文件的文件头信息，并将加密的数据块写入 ZIP 文件中。与无密码的 ZIP 文件不同，加密 ZIP 文件的文件头会标明文件已经加密，并记录加密所使用的算法等信息，以便在解压时提醒用户输入密码。

**文件头中的变化**：

​	•	文件头会标注该文件是否加密，以及采用的加密算法（如传统 ZIP 加密或 AES 加密）。

​	•	加密后的 ZIP 文件可以标记是否使用强加密算法（如 AES），这有助于解压程序识别并提示用户。

**4. 解压阶段的密码解密流程**

在解压加密的 ZIP 文件时，需要正确输入密码进行解密，具体过程如下：

​	•	**提示用户输入密码**：解压程序在读取 ZIP 文件头时会检测到该文件已被加密。此时会提示用户输入密码。

​	•	**生成解密密钥**：解压程序基于用户输入的密码生成解密密钥，使用的算法与压缩时一致（例如 AES）。

​	•	**验证密码正确性**：通过尝试解密一部分压缩文件的数据（通常是文件头的校验码），以确定密码是否正确。如果密码错误，解密数据会失败并提示用户重新输入。

​	•	**解密数据块**：当密码正确后，解压程序使用生成的密钥对加密的压缩数据块进行解密，恢复出压缩前的原始压缩数据。

​	•	**解压数据**：在成功解密后，解压程序会继续按常规的解压流程，将解密后的数据块进行解压缩，生成原始文件。

**5. 总结加密和解密的关键步骤**

**（1）压缩阶段的加密过程**：

​	•	文件压缩后，ZIP 工具使用用户提供的密码生成密钥，对压缩数据进行加密，并将加密的数据块写入 ZIP 文件。

​	•	文件头中标记该 ZIP 文件已加密，并记录所使用的加密算法。

**（2）解压阶段的解密过程**：

​	•	解压时，程序提示用户输入密码，通过该密码生成密钥，尝试解密加密的数据块。

​	•	密码正确后，程序恢复原始压缩数据，并继续进行解压操作。

### AES算法实现

**AES（Advanced Encryption Standard，高级加密标准）** 是一种对称加密算法，广泛应用于数据保护、文件加密等领域。AES 算法的特点是**高效、安全、易于实现**，它对数据进行分组加密，使用长度为 128 位、192 位或 256 位的密钥。以下是 AES 算法的具体实现过程。

**1. AES加密的基本概念**

​	•	**对称加密**：加密和解密使用相同的密钥，双方必须共享同一个密钥。

​	•	**分组加密**：AES 将数据按 128 位（16 字节）为一个分组进行加密，数据长度不足时需要填充。

​	•	**密钥长度**：AES 支持 128 位、192 位和 256 位的密钥长度。密钥越长，加密的安全性越高。

**2. AES算法的工作模式**

AES 算法由多个相同的**轮函数**（Rounds）组成，每一轮包括替换、混合、移位、加密密钥等操作。AES-128 需要 10 轮，AES-192 需要 12 轮，而 AES-256 则需要 14 轮。

每轮由四个主要步骤组成：

​	•	**字节替代（SubBytes）**：使用 S-box（替换表）对每个字节进行非线性替换，使得加密过程具备混淆性。

​	•	**行移位（ShiftRows）**：将数据块中的每一行字节按特定规则进行循环移位，增加混淆。

​	•	**列混合（MixColumns）**：将每一列进行线性混合操作，使数据在每个分组内的依赖关系更加复杂（此步骤在最后一轮中省略）。

​	•	**轮密钥加（AddRoundKey）**：将数据块与本轮密钥进行异或运算，每轮密钥是由原始密钥通过密钥扩展生成的。

**3. AES加密过程详细步骤**

**（1）密钥扩展**

​	•	AES 算法首先会根据输入的原始密钥（128 位、192 位或 256 位）生成一系列密钥，这些密钥被称为**轮密钥**（Round Key）。

​	•	扩展后的轮密钥将用于每一轮的加密和解密过程。

​	•	**密钥扩展算法**会将较短的原始密钥扩展成更长的密钥序列，长度取决于 AES 的轮数。

**（2）初始轮密钥加法**

​	•	在进入实际的加密轮次之前，AES 先将明文与初始密钥进行异或运算（XOR），这个操作叫做**AddRoundKey**。

​	•	这一步的目的是通过密钥将明文打乱。

**（3）10/12/14轮加密操作（具体步骤）**

​	•	**SubBytes（字节替代）**：每个字节通过查找预先定义的非线性替换表 S-box 替换为另一个字节。这一步增强了非线性，使加密变得更难预测。

​	•	**ShiftRows（行移位）**：数据块被组织为 4×4 的矩阵，每行字节按一定规则进行循环左移，行移位的目的是打乱数据块中的位置信息，使得不同位置的字节相互依赖。

​	•	第 1 行保持不变，第 2 行循环左移 1 个字节，第 3 行左移 2 个字节，第 4 行左移 3 个字节。

​	•	**MixColumns（列混合）**：每一列字节被线性变换为新的字节组合。这个步骤使用有限域 GF(2^8) 上的乘法和加法，将数据进一步混合，提高加密的扩散性。每个字节被其他字节影响，进一步复杂化加密。

​	•	**AddRoundKey（轮密钥加法）**：将当前数据块与对应的轮密钥进行异或运算（XOR），从而增加密钥与明文的关联性。

**（4）最后一轮加密**

​	•	在最后一轮中，省略了 **MixColumns（列混合）** 操作，只进行 **SubBytes**、**ShiftRows** 和 **AddRoundKey** 操作。

**（5）输出密文**

​	•	加密完成后，输出经过多轮加密后的密文。

**4. AES解密过程**

AES 的解密过程与加密过程是逆向操作，主要包括以下步骤：

**（1）密钥扩展**

​	•	与加密一样，首先进行密钥扩展，生成所有的轮密钥。

**（2）初始轮密钥加法**

​	•	将密文与最后一轮的轮密钥进行异或运算（AddRoundKey），恢复出经过最后一轮加密前的数据。

**（3）逆向操作**

​	•	每轮解密中，执行与加密相反的操作：

​	•	**InvShiftRows（逆行移位）**：与加密时相反，将各行字节循环右移。

​	•	**InvSubBytes（逆字节替代）**：使用逆向 S-box，将每个字节恢复为加密前的原始字节。

​	•	**InvMixColumns（逆列混合）**：对每列字节进行逆向线性变换，恢复加密前的字节顺序（这一操作在最后一轮中省略）。

​	•	**AddRoundKey**：将数据块与轮密钥异或还原。

**（4）输出明文**

​	•	完成所有轮次的解密后，最终输出解密后的明文，与加密前的原始数据一致。

**5. AES加密的工作模式**

AES 是分组加密算法，但可以结合多种工作模式来加密大块数据。常见的工作模式包括：

​	•	**ECB（电子密码本模式）**：直接对每个分组加密，简单但不安全，因为相同的明文块会产生相同的密文块。

​	•	**CBC（密码分组链接模式）**：使用前一个密文块加密当前分组，增强安全性，适用于大多数场合。

​	•	**CFB（密文反馈模式）** 和 **OFB（输出反馈模式）**：将分组加密转换为流加密模式，适合对连续数据流的加密。

​	•	**CTR（计数器模式）**：将加密过程并行化，提高效率，广泛应用于高性能环境。

**6. 总结**

​	•	**AES 的核心过程**包括字节替代、行移位、列混合和轮密钥加法，通过这些操作实现数据的混淆和扩散。

​	•	**AES 的安全性**依赖于轮数、密钥长度和加密算法的复杂性，通过密钥扩展和多轮运算，使得攻击者很难通过暴力破解或统计分析破解加密。

​	•	**AES 是目前国际上广泛应用的加密标准**，被广泛用于文件加密、网络传输、无线通信等领域。



### 暴力破解原理

**暴力破解**（Brute Force Attack）是一种最简单、但也是最直接的密码破解方法，它通过枚举所有可能的密码组合，直到找到正确的密码。这种方法不依赖密码的特定弱点，而是系统地尝试每一个可能的密码，因此可以在理论上破解任何密码加密系统，但效率非常低下，尤其当密码复杂度增加时。

**1. 暴力破解的原理**

暴力破解的核心原理是**穷举法**，即从一个密码空间中尝试所有可能的密码，直到解密成功。破解过程的步骤如下：

​	（1）获取需要破解的加密文件（如 ZIP 压缩包）。

​	（2）使用密码破解工具，开始从预定义的字符集（如小写字母、大写字母、数字、特殊符号等）中生成可能的密码组合。

​	（3）将每个生成的密码应用于目标加密文件，尝试解密。

​	（4）如果解密失败，继续尝试下一个密码，直到找到匹配的密码为止。

这个过程对加密算法没有要求，完全是基于密码本身的长度和复杂度，因此可以应用于各种类型的加密文件和系统。

**2. 密码空间与破解时间**

暴力破解的效率直接取决于密码的**长度**和**字符集的复杂度**。密码越长、字符集越大，暴力破解所需的时间越长。密码空间的大小可以通过以下公式计算：

例如：

​	•	如果密码由 26 个小写字母组成，且密码长度为 4，那么密码空间为 26^4 = 456,976 种可能的组合。

​	•	如果密码包含大小写字母和数字（62 个字符），且密码长度为 8，那么密码空间为 62^8=2.1834e+14 种可能的组合。

密码空间越大，破解所需的时间和计算资源就越多。

**破解时间的估算：**

破解时间可以通过密码空间和每秒尝试的密码数（**每秒尝试次数**）来估算：

假设破解工具每秒可以尝试 1000 个密码，某些例子的破解时间如下：

​	•	4 位小写字母密码空间为 456976，破解时间为：456976/1000=456.976秒，约为7.6分钟

​	•	8 位包含大小写字母和数字的密码空间为 ，破解时间约为：6926373/1440=4809.98125/365=13.17803年

这说明随着密码长度和字符集复杂度的增加，暴力破解的难度会迅速增加，导致所需时间呈指数增长。

**3. 影响暴力破解效率的因素**

**（1）密码长度**

密码越长，破解难度越大。每增加一个字符，密码空间会随着字符集的大小倍增。例如，在字符集为 62（大小写字母+数字）的情况下，增加一个字符会将密码空间扩展 62 倍，这极大增加了破解所需的时间。

**（2）字符集的复杂度**

​	•	**简单字符集**：如果密码只包含小写字母或数字（如“abc123”），密码空间较小，暴力破解所需的时间相对较短。

​	•	**复杂字符集**：如果密码包含大写字母、数字、特殊字符等（如“P@ssW0rd!”），字符集的大小增大，暴力破解所需时间大幅增长。例如：

​	•	小写字母字符集大小为 26。

​	•	小写字母+大写字母为 52。

​	•	小写字母+大写字母+数字为 62。

​	•	如果加上特殊符号，字符集可能达到 90 以上。

**（3）计算能力**

​	•	**CPU 性能**：暴力破解的速度主要取决于计算设备的性能。现代的多核 CPU 可以并行尝试多个密码组合，从而提高破解速度。

​	•	**GPU 加速**：GPU（图形处理器）具有大量的核心，能够并行处理海量数据，因此在暴力破解中被广泛使用。相比 CPU，GPU 可以更快地尝试更多的密码组合，尤其在处理较短的密码时优势明显。

​	•	**集群计算**：分布式计算（如利用云计算集群）可以进一步加速破解过程，通过多台设备并行进行密码尝试，从而显著缩短破解时间。

**（4）特定加密方式的强度**

不同的加密算法有不同的加密强度。现代加密算法如 AES，设计时已经考虑了抵抗暴力破解的能力。即使攻击者能在短时间内生成大量密码组合，也很难快速破解出密钥。加密算法越复杂，暴力破解所需时间越长。

**（5）常用工具的优化**

暴力破解工具（如 **John the Ripper** 和 **Hashcat**）不仅仅进行简单的暴力破解，还提供了一些优化功能：

​	•	**预计算优化**：通过优化计算流程，加快每秒密码尝试的速度。

​	•	**并行运算**：利用多个核心或多个计算设备并行进行破解尝试。

​	•	**字典攻击结合**：暴力破解和字典攻击结合使用，首先尝试常见的密码（字典），再在字典之外进行暴力穷举。

**4. 暴力破解的实际应用场景**

尽管暴力破解的效率低，但在某些情况下仍然可行，特别是以下几种场景：

​	•	**弱密码**：如果用户使用简单的密码（如短密码、常用密码组合），暴力破解可以在合理时间内找到正确密码。例如，破解包含 4~6 个字符的简单密码（如仅包含数字）通常是可以快速完成的。

​	•	**特定目标**：在某些特定目标中（如破解加密文件、破解密码保护的账号），攻击者可能会使用暴力破解来穷举可能的密码组合。

​	•	**密码重用**：很多用户会在不同平台上重复使用相同的密码。如果某一处密码泄露，攻击者可以通过暴力破解这些相似密码的变种来破解其他系统或文件的加密。



### 1234密码转化过程

为了详细描述 AES-128 算法中的一次**轮函数**（round function）过程，特别是针对密码 1234 进行加密时的转化过程，我们可以逐步讲解每个步骤，包括**字节替换**（SubBytes）、**行移位**（ShiftRows）、**列混合**（MixColumns）和**轮密钥加**（AddRoundKey）。



由于 1234 是一个短字符串，而 AES 的输入是一个 128 位（16 字节）的明文块，因此我们需要对 1234 进行适当的填充，并将其转化为 16 字节的块。此外，我们将 AES-128 算法的主要步骤简化为一次轮函数的过程（AES-128 总共 10 轮），详细介绍其中的一个轮次。



**AES 处理前的准备工作**



​	1.	**明文块准备：**

​	•	密码 “1234” 以 ASCII 编码表示为 0x31 0x32 0x33 0x34（对应的十六进制值），不足 16 字节的情况下，需要进行填充（通常使用 PKCS7 填充）。

​	•	经过填充后，最终明文变为：31 32 33 34 10 10 10 10 10 10 10 10 10 10 10 10 （其中 10 表示填充的字节，十六进制表示）。

​	2.	**初始密钥：**

AES-128 使用 128 位密钥，这里我们假设初始密钥为：2b7e151628aed2a6abf7158809cf4f3c。这将被用于密钥扩展（key expansion）和每一轮的轮密钥加法（AddRoundKey）。



**轮函数的四个主要步骤**



我们现在将说明 AES 的一次轮函数的四个步骤，它们包括：



​	1.	**字节替换（SubBytes）**

​	2.	**行移位（ShiftRows）**

​	3.	**列混合（MixColumns）**

​	4.	**轮密钥加（AddRoundKey）**



**1. 字节替换（SubBytes）**



**SubBytes** 是一个字节级的非线性变换，它通过查找**S盒**（Substitution Box）替换每个字节。AES 的 S 盒是预先定义的 16x16 的查找表，用于将每个字节替换为新的值。



对于我们的明文块 31 32 33 34 10 10 10 10 10 10 10 10 10 10 10 10：



​	•	明文中的第一个字节 0x31 对应在 S 盒中的位置，查表得出的替换值为 0x77。

​	•	明文中的第二个字节 0x32 通过 S 盒查表变为 0xB7。

​	•	明文中的第三个字节 0x33 通过 S 盒查表变为 0xFC。

​	•	以此类推，将每个字节通过 S 盒查表替换。



假设经过 S 盒替换后的状态为：



77 B7 FC 47

6D 6D 6D 6D

6D 6D 6D 6D

6D 6D 6D 6D



**2. 行移位（ShiftRows）**



**ShiftRows** 是一种行内字节循环移位操作，它对状态矩阵的每一行进行位移：



​	•	**第 1 行**保持不变。

​	•	**第 2 行**向左循环移位 1 字节。

​	•	**第 3 行**向左循环移位 2 字节。

​	•	**第 4 行**向左循环移位 3 字节。



举例说明，假设我们在字节替换（SubBytes）步骤后得到的状态矩阵是：



77 B7 FC 47

6D 6D 6D 6D

6D 6D 6D 6D

6D 6D 6D 6D



进行行移位后的状态矩阵为：



77 B7 FC 47  (第 1 行不变)

6D 6D 6D 6D  (第 2 行向左移 1 字节)

6D 6D 6D 6D  (第 3 行向左移 2 字节)

6D 6D 6D 6D  (第 4 行向左移 3 字节)



由于当前数据相同，行移位的结果在这特定例子中不会有显著变化。但对于不同数据组合，行移位可以引入更多的扩散效果。



**3. 列混合（MixColumns）**



**MixColumns** 是一项列操作，它将每一列中的四个字节混合在一起，目的是增加数据的扩散性。AES 将每一列看作一个 4 字节的向量，并通过与一个固定的矩阵进行有限域 GF(2^8) 上的乘法运算来进行混合。



MixColumns 的操作通过以下矩阵与状态矩阵的每一列相乘来实现：



[ 02 03 01 01 ]  [ s0 ]    [ s'0 ]

[ 01 02 03 01 ]  [ s1 ]  =  [ s'1 ]

[ 01 01 02 03 ] x [ s2 ]    [ s'2 ]

[ 03 01 01 02 ]  [ s3 ]    [ s'3 ]



其中，s0, s1, s2, s3 是每一列中的四个字节，经过乘法和异或运算后，得到新的一列 s'0, s'1, s'2, s'3。



假设经过 **MixColumns** 后，矩阵变为：



47 62 A3 C1

FF C1 A2 B3

3C 56 E1 92

A5 72 B6 4F



**4. 轮密钥加（AddRoundKey）**



**AddRoundKey** 是 AES 的核心步骤之一。它将状态矩阵与当前轮次的密钥进行按位异或（XOR）运算。每一轮的轮密钥是从初始密钥通过密钥扩展（Key Expansion）生成的。



假设当前轮的密钥为 23 A7 81 92 0F 38 4C D9 28 CF 5D 32 7A 16 49 53，将其转换成 4x4 的矩阵：



23 A7 81 92

0F 38 4C D9

28 CF 5D 32

7A 16 49 53



将此密钥与经过列混合后的状态矩阵按位异或：



状态矩阵:    47 62 A3 C1    密钥矩阵:    23 A7 81 92

​         FF C1 A2 B3           0F 38 4C D9

​         3C 56 E1 92           28 CF 5D 32

​         A5 72 B6 4F           7A 16 49 53



结果是：



状态矩阵 XOR 密钥矩阵 =



 47 ⊕ 23 = 64  62 ⊕ A7 = C5  A3 ⊕ 81 = 22  C1 ⊕ 92 = 53

 FF ⊕ 0F = F0  C1 ⊕ 38 = F9  A2 ⊕ 4C = EE  B3 ⊕ D9 = 6A

 3C ⊕ 28 = 14  56 ⊕ CF = 99  E1 ⊕ 5D = BC  92 ⊕ 32 = A0

 A5 ⊕ 7A = DF  72 ⊕ 16 = 64  B6 ⊕ 49 = FF  4F ⊕ 53 = 1C



因此，最终状态矩阵为：



64 C5 22 53

F0 F9 EE 6A

14 99 BC A0

DF 64 FF 1C



这个状态矩阵将作为下一轮轮函数的输入。



**总结**



在 AES-128 的一次轮函数中，我们详细讲解了每个步骤：



​	1.	**字节替换（SubBytes）**：通过 S 盒替换每个字节，使得非线性增加。

​	2.	**行移位（ShiftRows）**：对状态矩阵的每一行进行循环移位，增加扩散。

​	3.	**列混合（MixColumns）**：对每一列进行矩阵乘法，使得列中的字节相互混合，进一步增加扩散。

​	4.	**轮密钥加（AddRoundKey）**：将当前状态矩阵与轮密钥进行按位异或，结合密钥的影响。



这个过程在 AES-128 中会重复 10 轮（最后一轮不进行列混合），最终生成加密后的密文。

## 叙事逻辑

1. 我们要破解一个带密码的压缩包
2. 这个压缩包是如何加密的，为了防止被破解做了哪些事情
3. 为毛加密算法这么强了，我们还是能破解（我们使用的破解方式，绕开破解加密算法，而是选择暴力破解），但仍然有些局限性，不过可以碰碰运气
4. 给大家推荐的破解软件和使用方式

## 文本写作

### 开头

这，是你想下载的小黄油

打开后却发现，需要解压密码

得(dei)到指定的网站付费后获得

欲火焚身的你，会忍不住去购买吗？



这期视频，我会教你破解压缩包密码的方法



### 原理

在此之前，还需要明白加密者对压缩包做了什么？

#### 压缩原理——LZ算法

要使得小黄油压缩前和解压后，连毛都一样，就需要使用无损压缩

比如**LZ算法**(Lempel-Ziv)，它的原理非常简单

找到文件中重复的结构，并将它们简化

比方说有这么一组数据，源源源源源源宝宝宝宝宝宝宝宝宝，我们是不是一眼就知道该怎么压缩了，6个源9个宝，重复的次数加上字符本身，一下子就省了不少空间。

那有小伙伴要问了，加密的过程在哪里呢？

#### 加密原理——AES

最重要的加密阶段，发生在数据压缩之后，但在将数据写入最终 ZIP 文件之前。

可以使用我们小学二年级就学过的AES（Advanced Encryption Standard）

**它是被广泛使用的加密标准**，你家的路由器也用着它，无时无刻保护着你的网络隐私。

（算法后期动效建议参考https://www.bilibili.com/video/BV1yq4y1X7Kt/）

以AES-128说明，如果你设定压缩密码为ikun520

在 AES 处理前，ikun520会变成另一种鸽鸽的模样

也就是变为ASCII编码，并填充到一定长度（根据PKCS7填充标准）

最终转化为：

69 6B 75 6E 35 32 30 09 09 09 09 09 09 09 09 09（其中 09 表示填充的字节，长度16字节，十六进制表示）

这就是，明文块

（动效展示变成4x4矩阵）
$$
\begin{bmatrix}
69 & 6B & 75 & 6E \\
35 & 32 & 30 & 09 \\
09 & 09 & 09 & 09 \\
09 & 09 & 09 & 09
\end{bmatrix}
$$
是不是完全想象不到，你的密码在一瞬间，就变得面目全非了

可这还没结束

接下来，才正式进入AES的核心：

一开始，会替换一些数字，称为**字节替换**

将之前的矩阵，转化为映射到S盒中的值
$$
\begin{bmatrix}
D0 & BD & AC & B2 \\
18 & 30 & 50 & 3D \\
3D & 3D & 3D & 3D \\
3D & 3D & 3D & 3D
\end{bmatrix}
$$
类似于将中文翻译成对应的英文单词

这会让密码进行非线性变换，能更好抵御攻击者



接下来会基于一定的规律，移动矩阵中的后三行，称为**行移位**
$$
\begin{bmatrix}
D0 & BD & AC & B2 \\ 
30 & 50 & 3D & 18 \\
3D & 3D & 3D & 3D \\
3D & 3D & 3D & 3D
\end{bmatrix}
$$
这会让数据变得混淆，让攻击者难以破解



之后则是将行位移的结果与一个固定矩阵相乘，称为**列混合**

固定矩阵：
$$
\begin{bmatrix}
02 & 03 & 01 & 01 \\
01 & 02 & 03 & 01 \\
01 & 01 & 02 & 03 \\
03 & 01 & 01 & 02
\end{bmatrix}
$$
相乘结果：
$$
\begin{bmatrix}
67 & 5C & 8B & 22 \\
89 & E4 & 4F & D3 \\
F2 & 8A & C1 & 91 \\
54 & 9B & A7 & 46
\end{bmatrix}
$$
这会增强扩散性，让密文难以预测。



最后则是，将结果与一个特定密钥矩阵进行异或运算，称为**轮密钥加**

特定密钥矩阵
$$
\begin{bmatrix}
A0 & FA & FE & 17 \\
88 & A4 & CC & B1 \\
23 & 93 & D9 & 29 \\
2A & DF & 96 & A5
\end{bmatrix}
$$
进行逐位异或运算

以第一行举例

​	•	67 ⊕ a0 = C7

​	•	5C ⊕ fa = A6

​	•	8B ⊕ fe = 75

​	•	22 ⊕ 17 = 35

**得到第一轮，轮密钥加后的状态矩阵：**

C7 A6 75 35
01 40 83 62
D1 19 18 B8
7E 44 31 E3

这样的步骤还要进行10轮。

这个特定密钥机制较为复杂，因为篇幅有限，感兴趣的同学可以扣1，根据情况可以单独出一期视频讲解AES加密的详细流程与算法实现。

#### 破解原理——暴力破解

**AES 算法**本身是非常安全的，对于 **AES-128**，即使使用当今最强的超级计算机，以每秒尝试 10^18 个密钥的速度，也需要数十亿年才能暴力破解所有可能的密钥组合。

所以直接破解 AES 算法几乎是不可能的。

但是，我们可以采用暴力破解的方法，尝试用户设置的密码，特别是如果密码过短或过于简单。

暴力破解指的是，通过逐一尝试所有可能的密码组合，来猜测出正确密码

现代的多核 CPU 和 GPU 能够并行处理多个密码尝试，从而提高暴力破解的速度。

因此，如果密码强度不高，是能在有生之年被破解的。



### 破解工具分享

最后给大家分享两款主流的破解工具

#### ARCHPR

*（全称Advanced Archive Password Recovery）*

如果你是Windows用户，可以选择ARCHPR

它的解密功能十分强大，支持多种压缩文件

经过我的测试，解密出5位英文的密码，仅用时13秒

看到它的速度，你是否也和我一样，感动到湿掉了

它的具体使用步骤如下：

1. 选择要解密的压缩包
2. 选择“攻击类型”（如果不清楚，默认暴力即可）
3. 在“范围”中选择解密范围，可以先从全数字开始尝试，再逐步添加小写字母、大写字母等选项
4. 在“长度”中，选择口令长度，也就是密码长度范围，这个一般选10以下，10以上的也不好破解了
5. 最后点击顶栏的“开始！”

#### fcrackzip

（读音f-crack-zip）

对于Mac用户也有好用的工具——fcrackzip

使用homebrew在终端执行这行命令（brew install fcrackzip）进行安装

要破解加密的zip文件，只需执行这行命令（fcrackzip -v -b -c 'aA1' -l 1-10 -u ）

加上文件的绝对路径，即可

具体选项含义可以通过执行fcrackzip -h自行查看

测试同样的加密压缩包，十秒就可以搞定，非常带劲。



所有的链接已经放到评论区置顶中了，可以收藏下来，以备不时之需。

### 结尾

如果你想了解另一个使用GPU进行破解、号称世界最快的解密软件

请在公屏上打1

最后，如果你还想了解homebrew的安装过程，以及其它的包管理器

请在公屏上打2

我会优先制作相关视频。

如果本期视频对你有帮助的话

不要忘记长按点赞三连

我们下期见