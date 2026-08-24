import React, { useEffect, useRef, useState } from 'react';
import { motion } from 'framer-motion';
import { Github, Send, Terminal, Cpu, Database, Network } from 'lucide-react';

export default function HeroSection() {
  const canvasRef = useRef(null);
  const [mousePos, setMousePos] = useState({ x: 0, y: 0 });

  // Interactive Particle & Neural Core Canvas Engine
  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext('2d');
    
    let animationFrameId;
    let width = (canvas.width = canvas.offsetWidth);
    let height = (canvas.height = canvas.offsetHeight);

    const handleResize = () => {
      if (!canvas) return;
      width = canvas.width = canvas.offsetWidth;
      height = canvas.height = canvas.offsetHeight;
    };
    window.addEventListener('resize', handleResize);

    // Mouse tracking for parallax & reaction
    let targetX = width / 2;
    let targetY = height / 2;
    let currentX = width / 2;
    let currentY = height / 2;

    const handleMouseMove = (e) => {
      const rect = canvas.getBoundingClientRect();
      targetX = e.clientX - rect.left;
      targetY = e.clientY - rect.top;
      setMousePos({ x: targetX, y: targetY });
    };
    window.addEventListener('mousemove', handleMouseMove);

    // Particle initialization
    const particleCount = 65;
    const particles = Array.from({ length: particleCount }, () => ({
      x: Math.random() * width,
      y: Math.random() * height,
      vx: (Math.random() - 0.5) * 0.6,
      vy: (Math.random() - 0.5) * 0.6,
      radius: Math.random() * 2 + 1,
      baseAlpha: Math.random() * 0.5 + 0.2,
    }));

    let pulseAngle = 0;

    const render = () => {
      // Smooth mouse lerp
      currentX += (targetX - currentX) * 0.05;
      currentY += (targetY - currentY) * 0.05;

      ctx.clearRect(0, 0, width, height);

      const centerX = width * 0.72; // Positioned towards right side on desktop
      const centerY = height * 0.5;

      pulseAngle += 0.03;
      const pulseSize = Math.sin(pulseAngle) * 12;

      // Draw Neural Connections & Nodes
      particles.forEach((p, index) => {
        p.x += p.vx;
        p.y += p.vy;

        if (p.x < 0 || p.x > width) p.vx *= -1;
        if (p.y < 0 || p.y > height) p.vy *= -1;

        // Cursor magnetic interaction
        const dx = currentX - p.x;
        const dy = currentY - p.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < 120) {
          p.x -= (dx / dist) * 0.8;
          p.y -= (dy / dist) * 0.8;
        }

        // Draw particle dot
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(255, 107, 0, ${p.baseAlpha})`;
        ctx.shadowColor = '#FF6B00';
        ctx.shadowBlur = 8;
        ctx.fill();

        // Connect nearby nodes
        for (let j = index + 1; j < particles.length; j++) {
          const p2 = particles[j];
          const distance = Math.hypot(p.x - p2.x, p.y - p2.y);
          if (distance < 100) {
            ctx.beginPath();
            ctx.moveTo(p.x, p.y);
            ctx.lineTo(p2.x, p2.y);
            ctx.strokeStyle = `rgba(255, 107, 0, ${0.15 * (1 - distance / 100)})`;
            ctx.lineWidth = 0.75;
            ctx.stroke();
          }
        }

        // Connect to central AI core
        const coreDist = Math.hypot(p.x - centerX, p.y - centerY);
        if (coreDist < 160) {
          ctx.beginPath();
          ctx.moveTo(p.x, p.y);
          ctx.lineTo(centerX, centerY);
          ctx.strokeStyle = `rgba(255, 107, 0, ${0.12 * (1 - coreDist / 160)})`;
          ctx.lineWidth = 0.5;
          ctx.stroke();
        }
      });

      // Draw Glowing Central AI Core / Neural Sphere
      const gradient = ctx.createRadialGradient(centerX, centerY, 0, centerX, centerY, 70 + pulseSize);
      gradient.addColorStop(0, 'rgba(255, 107, 0, 0.35)');
      gradient.addColorStop(0.5, 'rgba(255, 107, 0, 0.1)');
      gradient.addColorStop(1, 'rgba(0, 0, 0, 0)');

      ctx.beginPath();
      ctx.arc(centerX, centerY, 80 + pulseSize, 0, Math.PI * 2);
      ctx.fillStyle = gradient;
      ctx.fill();

      // Core center solid ring
      ctx.beginPath();
      ctx.arc(centerX, centerY, 24, 0, Math.PI * 2);
      ctx.strokeStyle = '#FF6B00';
      ctx.lineWidth = 2;
      ctx.shadowColor = '#FF6B00';
      ctx.shadowBlur = 15;
      ctx.stroke();

      animationFrameId = requestAnimationFrame(render);
    };

    render();

    return () => {
      window.removeEventListener('resize', handleResize);
      window.removeEventListener('mousemove', handleMouseMove);
      cancelAnimationFrame(animationFrameId);
    };
  }, []);

  return (
    <section className="relative w-full h-screen min-h-[700px] bg-[#0A0A0A] text-white overflow-hidden flex items-center justify-center font-sans select-none">
      
      {/* Background Ambient Glow & Grid Pattern */}
      <div className="absolute inset-0 bg-[radial-gradient(circle_at_70%_50%,rgba(255,107,0,0.08),transparent_50%)] pointer-events-none" />
      <div className="absolute inset-0 bg-[linear-gradient(to_right,#141414_1px,transparent_1px),linear-gradient(to_bottom,#141414_1px,transparent_1px)] bg-[size:4rem_4rem] [mask-image:radial-gradient(ellipse_60%_50%_at_50%_50%,#000_70%,transparent_100%)] opacity-25 pointer-events-none" />

      {/* Interactive Canvas Visual (Neural Core & Particles) */}
      <canvas
        ref={canvasRef}
        className="absolute inset-0 w-full h-full z-10 pointer-events-none"
      />

      {/* Futuristic AI HUD Labels (Subtle UI Floating Elements) */}
      <div className="absolute inset-0 z-10 pointer-events-none hidden lg:block">
        <div className="absolute top-[28%] right-[22%] flex items-center gap-2 text-[10px] tracking-widest text-[#FF6B00]/70 font-mono uppercase bg-black/40 px-2.5 py-1 rounded border border-[#FF6B00]/20 backdrop-blur-sm">
          <Cpu className="w-3 h-3 text-[#FF6B00]" /> AI / ML Core
        </div>
        <div className="absolute top-[62%] right-[15%] flex items-center gap-2 text-[10px] tracking-widest text-[#FF6B00]/70 font-mono uppercase bg-black/40 px-2.5 py-1 rounded border border-[#FF6B00]/20 backdrop-blur-sm">
          <Network className="w-3 h-3 text-[#FF6B00]" /> Neural Network
        </div>
        <div className="absolute top-[20%] right-[42%] flex items-center gap-2 text-[10px] tracking-widest text-[#FF6B00]/70 font-mono uppercase bg-black/40 px-2.5 py-1 rounded border border-[#FF6B00]/20 backdrop-blur-sm">
          <Database className="w-3 h-3 text-[#FF6B00]" /> System Online
        </div>
      </div>

      {/* Main Content Layout */}
      <div className="relative z-20 max-w-7xl w-full mx-auto px-6 sm:px-12 lg:px-16 flex flex-col lg:flex-row items-center justify-between">
        
        <div className="max-w-2xl flex flex-col items-start space-y-6 pt-12 lg:pt-0">
          
          {/* Status Indicator */}
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.5 }}
            className="flex items-center gap-2 px-3 py-1 rounded-full bg-[#161616] border border-[#262626] text-xs font-mono text-zinc-400"
          >
            <span className="w-2 h-2 rounded-full bg-[#FF6B00] animate-pulse shadow-[0_0_8px_#FF6B00]" />
            <span>PORTFOLIO_V2.6 // LIVE</span>
          </motion.div>

          {/* Hierarchy 1: Name */}
          <motion.h1 
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6, delay: 0.1 }}
            className="text-4xl sm:text-6xl lg:text-7xl font-extrabold tracking-tight text-white leading-[1.1]"
          >
            Hi, I'm <span className="text-transparent bg-clip-text bg-gradient-to-r from-white via-zinc-200 to-[#FF6B00]">Sanyukt Kumar Rai</span>
          </motion.h1>

          {/* Hierarchy 2: Developer Identity */}
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6, delay: 0.2 }}
            className="flex flex-wrap items-center gap-2 text-sm sm:text-base font-mono text-[#FF6B00] font-medium"
          >
            <span>Full Stack Developer</span>
            <span className="text-zinc-600">•</span>
            <span>AI/ML Enthusiast</span>
            <span className="text-zinc-600">•</span>
            <span>Graphics Designer</span>
          </motion.div>

          {/* Hierarchy 4: Short Description */}
          <motion.p
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6, delay: 0.3 }}
            className="text-base sm:text-lg text-zinc-400 font-normal max-w-lg leading-relaxed"
          >
            Building intelligent, useful and visually engaging digital experiences at the intersection of code and generative creative technology.
          </motion.p>

          {/* Hierarchy 5: CTA Buttons */}
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6, delay: 0.4 }}
            className="flex flex-wrap items-center gap-4 pt-2"
          >
            <a
              href="https://github.com/Sanyukt-Kumar-Rai"
              target="_blank"
              rel="noopener noreferrer"
              className="flex items-center gap-2.5 px-6 py-3.5 rounded-xl bg-[#FF6B00] text-black font-semibold text-sm hover:bg-[#e05f00] transition-all duration-300 shadow-[0_0_20px_rgba(255,107,0,0.3)] hover:shadow-[0_0_25px_rgba(255,107,0,0.5)] cursor-pointer"
            >
              <Github className="w-4 h-4 fill-current" />
              Explore My GitHub
            </a>

            <a
              href="#contact"
              className="flex items-center gap-2.5 px-6 py-3.5 rounded-xl bg-[#141414] hover:bg-[#1f1f1f] text-zinc-200 border border-[#2b2b2b] hover:border-[#FF6B00]/50 font-semibold text-sm transition-all duration-300 cursor-pointer backdrop-blur-md"
            >
              <Send className="w-4 h-4 text-[#FF6B00]" />
              Connect With Me
            </a>
          </motion.div>

        </div>

        {/* Spacer column to keep the right side open for the immersive canvas core */}
        <div className="hidden lg:block lg:w-[40%] h-[400px]" />

      </div>

      {/* Bottom Subtle Gradient Fade */}
      <div className="absolute bottom-0 left-0 w-full h-24 bg-gradient-to-t from-[#0A0A0A] to-transparent pointer-events-none" />
    </section>
  );
}
