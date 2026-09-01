---
description: "Welcome to Knowledge Intelligence Lab (KILab) at Sun Yat-sen University"
---

{{< alert "circle-info" >}}
**Welcome to the Knowledge Intelligence Lab (KILab) !**

Our lab is directed by Dr. Ziyu Lyu. We are dedicated to advancing cutting-edge research in Knowledge Intelligence. We are specified in Secure and Trusted AI, Intelligent Information Retrieval, Natural Language Processing. Recently, we are focusing on AIGC Safety, Social Media Content Safety and Reliable Recommender System.

<span style="color: red;">We are actively recruiting Postdocs, PhD students, Master students,and research interns. Welcome to Join us!</span>
{{< /alert >}}



## Featured Projects

<div class="project-carousel">
  <div class="carousel-container">
    <!-- AdaCD Slide -->
    <div class="carousel-slide active" data-slide="0">
      <div class="slide-image">
        <img src="/posts/adacd/feature.png" alt="AdaCD Project" />
      </div>
      <div class="slide-content">
        <div class="project-badge">🛡️ AI Safety</div>
        <h2>AdaCD</h2>
        <p>Adaptive Contrastive Decoding to mitigate over-refusal while preserving LLM safety</p>
        <div class="project-links">
          <a href="/posts/adacd/" class="btn btn-primary">📄 Post</a>
          <a href="https://github.com/OutdoorManofML/AdaCD" target="_blank" class="btn btn-secondary">💻 GitHub</a>
        </div>
      </div>
    </div>
    <!-- D-Judge Slide -->
    <div class="carousel-slide" data-slide="1">
      <div class="slide-image">
        <img src="/projects/D-judge/1.png" alt="D-Judge Project" />
      </div>
      <div class="slide-content">
        <div class="project-badge">🔬 Computer Vision</div>
        <h2>D-Judge</h2>
        <p>A Large-Scale Dataset for Visual Research on AI-Synthesized and Natural Images</p>
        <div class="project-links">
          <a href="https://arxiv.org/abs/2412.17632" target="_blank" class="btn btn-primary">📄 arXiv</a>
          <a href="https://huggingface.co/datasets/Renyang/DANI" target="_blank" class="btn btn-secondary">🤗 Dataset</a>
          <a href="https://github.com/ryliu68/DJudge" target="_blank" class="btn btn-secondary">💻 GitHub</a>
        </div>
      </div>
    </div>
    <!-- MidPO Slide -->
    <div class="carousel-slide" data-slide="2">
      <div class="slide-image">
        <img src="/projects/MidPO/1.png" alt="MidPO Project" />
      </div>
      <div class="slide-content">
        <div class="project-badge">⚡ AI Safety</div>
        <h2>MidPO</h2>
        <p>Dual Preference Optimization for Safety and Helpfulness in LLMs via MoE Framework</p>
        <div class="project-links">
          <a href="https://arxiv.org/abs/2506.02460" target="_blank" class="btn btn-primary">📄 arXiv</a>
          <a href="#" class="btn btn-secondary disabled">💻 Code (Soon)</a>
        </div>
      </div>
    </div>
    <!-- FairWork Slide -->
    <div class="carousel-slide" data-slide="3">
      <div class="slide-image">
        <img src="/projects/FairWork/1.png" alt="FairWork Project" />
      </div>
      <div class="slide-content">
        <div class="project-badge">⚖️ Fairness</div>
        <h2>FairWork</h2>
        <p>Fairness-aware Work Allocation and Assessment Demo</p>
        <div class="project-links">
          <a href="https://huggingface.co/spaces/chenzhouliiii/FairWork2" target="_blank" class="btn btn-primary">🎮 Demo</a>
        </div>
      </div>
    </div>

  </div>
  
  <!-- Navigation Indicators -->
  <div class="carousel-indicators">
    <button class="indicator active" data-slide="0" aria-label="Go to AdaCD"></button>
    <button class="indicator" data-slide="1" aria-label="Go to D-Judge"></button>
    <button class="indicator" data-slide="2" aria-label="Go to MidPO"></button>
    <button class="indicator" data-slide="3" aria-label="Go to FairWork"></button>
  </div>
</div>



{{< alert "lightbulb" >}}
[2026.4] Congrats to **Jin Zeng**! Our work, "RAIE: Region-Aware Incremental Preference Editing with LoRA for LLM-based Recommendation", has been accepted by **WWW 2026**.
{{< /alert >}}

{{< alert "lightbulb" >}}
[2026.4] Congrats to **Yupeng Qi**! Our work, "Please Refuse to Answer Me! Mitigating Over-Refusal in LLMs via Adaptive Contrastive Decoding", has been accepted by **ACL 2026 Main Conference**.
{{< /alert >}}

{{< alert "lightbulb" >}}
[2025.8] Congrats to **Yupeng Qi**! Our work, "MidPO: Dual Preference Optimization for Safety and Helpfulness in LLMs via MoE Framework", has been accepted by **EMNLP FINDING 2025**.
{{< /alert >}}

{{< alert "lightbulb" >}}
[2025.7] Congrats to **[Renyang Liu](https://scholar.google.com/citations?user=yUJafNAAAAAJ&hl=en)**! Our collaborative work, "D-Judge: How Far Are We? Accessing the Discrepancies Between AI-synthesized Images and Natural Images through Multimodal Guidance", has been accepted by **ACM MM 2025**.
{{< /alert >}}





<style>
.project-carousel {
  max-width: 1200px;
  margin: 40px auto;
  position: relative;
  background: #f8f9fa;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
}

.carousel-container {
  position: relative;
  height: 350px;
}

.carousel-slide {
  display: none;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease-in-out;
}

.carousel-slide.active {
  display: grid;
  opacity: 1;
}

.slide-image {
  padding: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.slide-image img {
  max-width: 100%;
  max-height: 250px;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.15);
  object-fit: contain;
}

.slide-content {
  padding: 30px;
}

.project-badge {
  display: inline-block;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 16px;
}

.slide-content h2 {
  font-size: 2.5rem;
  font-weight: bold;
  margin: 0 0 16px 0;
  color: #333;
}

.slide-content p {
  font-size: 1.1rem;
  color: #666;
  line-height: 1.6;
  margin-bottom: 24px;
}

.project-links {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.btn {
  padding: 12px 24px;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 600;
  transition: all 0.2s ease;
  border: none;
  cursor: pointer;
  font-size: 14px;
}

.btn-primary {
  background: #007bff;
  color: white;
}

.btn-primary:hover {
  background: #0056b3;
  transform: translateY(-1px);
}

.btn-secondary {
  background: #6c757d;
  color: white;
}

.btn-secondary:hover {
  background: #545b62;
  transform: translateY(-1px);
}

.btn.disabled {
  background: #e9ecef;
  color: #6c757d;
  cursor: not-allowed;
}

.carousel-indicators {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 12px;
}

.indicator {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  border: none;
  background: rgba(255,255,255,0.5);
  cursor: pointer;
  transition: all 0.3s ease;
}

.indicator.active {
  background: #007bff;
  transform: scale(1.2);
}

.indicator:hover {
  background: rgba(255,255,255,0.8);
}

/* Responsive Design */
@media (max-width: 768px) {
  .carousel-slide.active {
    display: block;
  }
  
  .carousel-container {
    height: auto;
  }
  
  .slide-image {
    padding: 20px 20px 0 20px;
  }
  
  .slide-content {
    padding: 20px;
  }
  
  .slide-content h2 {
    font-size: 2rem;
  }
  
  .project-links {
    justify-content: center;
  }
}

@media (max-width: 480px) {
  .slide-content h2 {
    font-size: 1.5rem;
  }
  
  .btn {
    padding: 10px 16px;
    font-size: 12px;
  }
}
</style>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const carousel = document.querySelector('.project-carousel');
  if (!carousel) return;
  
  const slides = carousel.querySelectorAll('.carousel-slide');
  const indicators = carousel.querySelectorAll('.indicator');
  let currentSlide = 0;
  let autoplayInterval;
  let isHovered = false;
  
  // Show specific slide
  function showSlide(index) {
    // Hide all slides
    slides.forEach(slide => {
      slide.classList.remove('active');
    });
    
    // Remove active from all indicators
    indicators.forEach(indicator => {
      indicator.classList.remove('active');
    });
    
    // Show current slide and indicator
    if (slides[index]) {
      slides[index].classList.add('active');
    }
    if (indicators[index]) {
      indicators[index].classList.add('active');
    }
    
    currentSlide = index;
  }
  
  // Go to next slide
  function nextSlide() {
    const next = (currentSlide + 1) % slides.length;
    showSlide(next);
  }
  
  // Start autoplay
  function startAutoplay() {
    autoplayInterval = setInterval(() => {
      if (!isHovered) {
        nextSlide();
      }
    }, 2000); // Change slide every 5 seconds
  }
  
  // Stop autoplay
  function stopAutoplay() {
    if (autoplayInterval) {
      clearInterval(autoplayInterval);
    }
  }
  
  // Add click listeners to indicators
  indicators.forEach((indicator, index) => {
    indicator.addEventListener('click', () => {
      showSlide(index);
      stopAutoplay();
      startAutoplay(); // Restart autoplay timer
    });
  });
  
  // Pause autoplay on hover
  carousel.addEventListener('mouseenter', () => {
    isHovered = true;
  });
  
  carousel.addEventListener('mouseleave', () => {
    isHovered = false;
  });
  
  // Add keyboard navigation
  document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowLeft') {
      const prev = currentSlide === 0 ? slides.length - 1 : currentSlide - 1;
      showSlide(prev);
      stopAutoplay();
      startAutoplay();
    } else if (e.key === 'ArrowRight') {
      nextSlide();
      stopAutoplay();
      startAutoplay();
    }
  });
  
  // Initialize carousel
  showSlide(0);
  startAutoplay();
});
</script>
