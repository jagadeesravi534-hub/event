// ============================================
// MOBILE MENU TOGGLE
// ============================================

const hamburger = document.querySelector('.hamburger');
const navLinks = document.querySelector('.nav-links');

if (hamburger) {
    hamburger.addEventListener('click', () => {
        navLinks.style.display = navLinks.style.display === 'flex' ? 'none' : 'flex';
        navLinks.style.position = 'absolute';
        navLinks.style.top = '60px';
        navLinks.style.left = '0';
        navLinks.style.right = '0';
        navLinks.style.flexDirection = 'column';
        navLinks.style.backgroundColor = 'white';
        navLinks.style.boxShadow = '0 10px 30px rgba(0, 0, 0, 0.1)';
        navLinks.style.padding = '20px';
        navLinks.style.gap = '15px';
        navLinks.style.zIndex = '999';
    });
}

// Close menu when a link is clicked
const navItems = document.querySelectorAll('.nav-links a');
navItems.forEach(item => {
    item.addEventListener('click', () => {
        if (navLinks.style.display === 'flex') {
            navLinks.style.display = 'none';
        }
    });
});

// ============================================
// ATTEND BUTTON FUNCTIONALITY
// ============================================

const attendButtons = document.querySelectorAll('.attend-btn');
attendButtons.forEach(button => {
    button.addEventListener('click', (e) => {
        e.preventDefault();
        const eventCard = button.closest('.event-card');
        const eventTitle = eventCard.querySelector('h3').textContent;
        
        // Show confirmation
        const originalText = button.textContent;
        button.textContent = '✓ Added';
        button.style.background = 'linear-gradient(135deg, #2ecc71, #27ae60)';
        
        // Revert after 2 seconds
        setTimeout(() => {
            button.textContent = originalText;
            button.style.background = '';
        }, 2000);

        // Show notification
        showNotification(`You've been added to "${eventTitle}"`);
    });
});

// ============================================
// EXPLORE EVENTS BUTTON
// ============================================

const ctaButton = document.querySelector('.cta-button');
if (ctaButton) {
    ctaButton.addEventListener('click', () => {
        const eventsSection = document.getElementById('events');
        if (eventsSection) {
            eventsSection.scrollIntoView({ behavior: 'smooth' });
        }
    });
}

// ============================================
// NOTIFICATION SYSTEM
// ============================================

function showNotification(message) {
    const notification = document.createElement('div');
    notification.style.cssText = `
        position: fixed;
        top: 100px;
        right: 20px;
        background: linear-gradient(135deg, #667eea, #764ba2);
        color: white;
        padding: 15px 25px;
        border-radius: 10px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        z-index: 10000;
        animation: slideIn 0.5s ease;
        font-weight: 500;
    `;
    notification.textContent = message;
    document.body.appendChild(notification);

    setTimeout(() => {
        notification.style.animation = 'slideOut 0.5s ease';
        setTimeout(() => notification.remove(), 500);
    }, 3000);
}

// ============================================
// NEWSLETTER FORM
// ============================================

const newsletterForm = document.querySelector('.newsletter-form');
if (newsletterForm) {
    newsletterForm.addEventListener('submit', (e) => {
        e.preventDefault();
        const email = newsletterForm.querySelector('input[type="email"]').value;
        if (email) {
            showNotification(`✓ Subscribed with ${email}`);
            newsletterForm.reset();
        }
    });
}

// ============================================
// CONTACT FORM
// ============================================

const contactForm = document.querySelector('.contact-form');
if (contactForm) {
    contactForm.addEventListener('submit', (e) => {
        e.preventDefault();
        const name = contactForm.querySelector('input[type="text"]').value;
        showNotification(`✓ Message sent! We'll contact you soon, ${name}.`);
        contactForm.reset();
    });
}

// ============================================
// SCROLL ANIMATIONS
// ============================================

const observerOptions = {
    threshold: 0.1,
    rootMargin: '0px 0px -50px 0px'
};

const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.style.animation = 'fadeInUp 0.6s ease forwards';
            observer.unobserve(entry.target);
        }
    });
}, observerOptions);

// Observe event cards and feature cards
document.querySelectorAll('.event-card, .feature-card').forEach(card => {
    card.style.opacity = '0';
    observer.observe(card);
});

// ============================================
// SMOOTH SCROLLING FOR NAVIGATION
// ============================================

document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function (e) {
        e.preventDefault();
        const target = document.querySelector(this.getAttribute('href'));
        if (target) {
            target.scrollIntoView({
                behavior: 'smooth',
                block: 'start'
            });
        }
    });
});

// ============================================
// NAVBAR TRANSPARENCY ON SCROLL
// ============================================

const navbar = document.querySelector('.navbar');
window.addEventListener('scroll', () => {
    if (window.scrollY > 50) {
        navbar.style.boxShadow = '0 10px 30px rgba(0, 0, 0, 0.15)';
    } else {
        navbar.style.boxShadow = '0 10px 30px rgba(0, 0, 0, 0.1)';
    }
});

// ============================================
// ANIMATIONS KEYFRAMES (CSS-in-JS)
// ============================================

const style = document.createElement('style');
style.textContent = `
    @keyframes slideIn {
        from {
            transform: translateX(100%);
            opacity: 0;
        }
        to {
            transform: translateX(0);
            opacity: 1;
        }
    }

    @keyframes slideOut {
        from {
            transform: translateX(0);
            opacity: 1;
        }
        to {
            transform: translateX(100%);
            opacity: 0;
        }
    }

    @keyframes fadeInUp {
        from {
            opacity: 0;
            transform: translateY(30px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }
`;
document.head.appendChild(style);

// ============================================
// PARALLAX EFFECT (OPTIONAL)
// ============================================

const hero = document.querySelector('.hero');
if (hero) {
    window.addEventListener('scroll', () => {
        const scrolled = window.pageYOffset;
        const animatedBg = document.querySelector('.animated-bg');
        if (animatedBg) {
            animatedBg.style.transform = `translateY(${scrolled * 0.5}px)`;
        }
    });
}

// ============================================
// EVENT COUNTER
// ============================================

function animateCounter(element, target, duration = 1000) {
    let current = 0;
    const increment = target / (duration / 16);
    
    const timer = setInterval(() => {
        current += increment;
        if (current >= target) {
            element.textContent = target;
            clearInterval(timer);
        } else {
            element.textContent = Math.floor(current);
        }
    }, 16);
}

// Trigger counter animation when featured events section is visible
const featuredSection = document.querySelector('.events');
let countersAnimated = false;

if (featuredSection) {
    const counterObserver = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting && !countersAnimated) {
                countersAnimated = true;
                // You can add counter animations here if needed
            }
        });
    }, { threshold: 0.5 });
    
    counterObserver.observe(featuredSection);
}

// ============================================
// PAGE LOAD ANIMATION
// ============================================

window.addEventListener('load', () => {
    document.body.style.animation = 'fadeInUp 0.6s ease';
});

// ============================================
// RESPONSIVE MENU CLOSE ON RESIZE
// ============================================

window.addEventListener('resize', () => {
    if (window.innerWidth > 768 && navLinks) {
        navLinks.style.display = 'flex';
        navLinks.style.position = 'relative';
        navLinks.style.top = 'auto';
        navLinks.style.left = 'auto';
        navLinks.style.backgroundColor = 'transparent';
        navLinks.style.boxShadow = 'none';
        navLinks.style.padding = '0';
        navLinks.style.flexDirection = 'row';
        navLinks.style.gap = '40px';
        navLinks.style.zIndex = 'auto';
    }
});

// ============================================
// CONSOLE WELCOME MESSAGE
// ============================================

console.log('%c🎉 Welcome to EventHub!', 'font-size: 24px; color: #667eea; font-weight: bold;');
console.log('%cDiscover amazing events and create unforgettable memories!', 'font-size: 14px; color: #764ba2;');
