# Computer Vision Study Notes

## 폴더별 구성

### Vision_model

| Notebook      | 논문                                                                               | 연도             | 비고                       |
| ------------- | ---------------------------------------------------------------------------------- | ---------------- | -------------------------- |
| `VGG16.ipynb` | _Very Deep Convolutional Networks for Large-Scale Image Recognition_               | 2014             | VGG16 구현                 |
| `cam.ipynb`   | _Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization_ | 2016 / ICCV 2017 | 노트북 내부 구현 기준 확인 |

### Generative_model

| Notebook               | 논문                                                                              | 연도 | 비고                                |
| ---------------------- | --------------------------------------------------------------------------------- | ---- | ----------------------------------- |
| `VanillaGAN.ipynb`     | _Generative Adversarial Nets_                                                     | 2014 | 기본 GAN                            |
| `conditionalGAN.ipynb` | _Conditional Generative Adversarial Nets_                                         | 2014 | 조건부 GAN                          |
| `pix2pix.ipynb`        | _Image-to-Image Translation with Conditional Adversarial Networks_                | 2017 | paired image-to-image translation   |
| `CycleGAN.ipynb`       | _Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks_ | 2017 | unpaired image-to-image translation |

## Vision_model

### 1. VGG16

- 논문: _Very Deep Convolutional Networks for Large-Scale Image Recognition_
- 저자: Karen Simonyan, Andrew Zisserman
- 핵심 아이디어:
  - 작은 `3x3` convolution을 여러 층 깊게 쌓아 표현력을 높인다.
  - 네트워크 깊이 증가가 대규모 이미지 분류 성능 향상에 중요하다는 점을 보여준다.
  - VGG16/VGG19 구조가 이후 CNN 백본의 기준점 역할을 했다.
- 노트북과의 연결:
  - `VGG16.ipynb`는 VGG block을 직접 구성해 분류 모델을 학습하는 형태다.
  - ImageNet 원논문 구조를 바탕으로, 노트북에서는 CIFAR-10 등 더 작은 데이터셋에 맞게 일부 출력층을 조정한 구현으로 보인다.
- 링크:
  - arXiv: https://arxiv.org/abs/1409.1556

### 2. Grad-CAM

- 논문: _Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization_
- 저자: Ramprasaath R. Selvaraju, Michael Cogswell, Abhishek Das, Ramakrishna Vedantam, Devi Parikh, Dhruv Batra
- 핵심 아이디어:
  - 특정 클래스 점수에 대한 gradient를 마지막 convolution feature map에 역전파해 중요한 영역을 시각화한다.
  - 모델 구조를 크게 바꾸지 않고 설명 가능한 시각화를 제공한다.
  - 분류뿐 아니라 captioning, VQA 등에도 확장 가능하다.
- 노트북과의 연결:
  - `cam.ipynb` 내부에 `Grad_CAM_heatmap.jpg`, `Guided_Grad_CAM.jpg` 생성 코드가 있어 CAM이 아니라 Grad-CAM 구현으로 판단했다.
  - VGG 계열 분류기에 대해 heatmap을 생성하는 흐름이다.
- 링크:
  - arXiv: https://arxiv.org/abs/1610.02391
  - ICCV 2017: https://openaccess.thecvf.com/content_ICCV_2017/html/Selvaraju_Grad-CAM_Visual_Explanations_ICCV_2017_paper.html

## Generative_model

### 1. Vanilla GAN

- 논문: _Generative Adversarial Nets_
- 저자: Ian J. Goodfellow, Jean Pouget-Abadie, Mehdi Mirza, Bing Xu, David Warde-Farley, Sherjil Ozair, Aaron Courville, Yoshua Bengio
- 핵심 아이디어:
  - Generator와 Discriminator를 적대적으로 학습시켜 데이터 분포를 모사한다.
  - 생성 모델 학습을 미분 가능한 게임 형태로 정식화했다.
  - 이후 거의 모든 GAN 계열 연구의 출발점이 된 논문이다.
- 노트북과의 연결:
  - `VanillaGAN.ipynb`는 가장 기본적인 generator/discriminator 학습 구조를 다룬다.
  - 조건 정보나 cycle consistency 없이 순수 GAN 학습을 다루는 형태다.
- 링크:
  - NeurIPS 2014 PDF: https://proceedings.neurips.cc/paper_files/paper/2014/file/f033ed80deb0234979a61f95710dbe25-Paper.pdf

### 2. Conditional GAN

- 논문: _Conditional Generative Adversarial Nets_
- 저자: Mehdi Mirza, Simon Osindero
- 핵심 아이디어:
  - 생성기와 판별기에 label 또는 다른 조건 정보 `y`를 함께 입력해 조건부 생성을 수행한다.
  - 원하는 클래스나 조건을 통제하면서 샘플을 생성할 수 있다.
  - 이후 pix2pix 같은 조건부 생성 모델의 직접적인 기반이 됐다.
- 노트북과의 연결:
  - `conditionalGAN.ipynb`는 label-conditioned generation 구조를 구현한 노트북으로 보인다.
  - 기본 GAN 대비 입력 조건을 추가해 특정 클래스 기반 생성을 수행하는 전형적인 cGAN 흐름이다.
- 링크:
  - arXiv: https://arxiv.org/abs/1411.1784

### 3. pix2pix

- 논문: _Image-to-Image Translation with Conditional Adversarial Networks_
- 저자: Phillip Isola, Jun-Yan Zhu, Tinghui Zhou, Alexei A. Efros
- 핵심 아이디어:
  - paired dataset이 있을 때 입력 이미지를 목표 이미지로 변환하는 image-to-image translation 프레임워크다.
  - U-Net generator와 PatchGAN discriminator 조합이 널리 알려졌다.
  - adversarial loss와 reconstruction loss를 함께 사용한다.
- 노트북과의 연결:
  - `pix2pix.ipynb`는 `cityscapes` paired dataset을 이용하는 코드가 포함되어 있다.
  - 이는 원논문의 대표 실험 설정과 직접 연결된다.
- 링크:
  - CVPR 2017 HTML/PDF: https://openaccess.thecvf.com/content_cvpr_2017/html/Isola_Image-To-Image_Translation_With_CVPR_2017_paper.html

### 4. CycleGAN

- 논문: _Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks_
- 저자: Jun-Yan Zhu, Taesung Park, Phillip Isola, Alexei A. Efros
- 핵심 아이디어:
  - paired data 없이도 두 도메인 간 변환을 학습한다.
  - `X -> Y`, `Y -> X` 두 generator와 cycle consistency loss를 사용한다.
  - 말 사진을 얼룩말로 바꾸는 예시처럼 비정렬 도메인 변환으로 유명하다.
- 노트북과의 연결:
  - `CycleGAN.ipynb`는 residual block 기반 generator와 양방향 변환 구조를 구현하고 있다.
  - paired image 없이 도메인 변환을 학습하는 CycleGAN의 핵심 설계를 반영한다.
- 링크:
  - arXiv: https://arxiv.org/abs/1703.10593
  - ICCV 2017: https://openaccess.thecvf.com/content_ICCV_2017/html/Zhu_Unpaired_Image-To-Image_Translation_ICCV_2017_paper.html
