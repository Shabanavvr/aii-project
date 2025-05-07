// server.js - Express backend for Bridge platform
const express = require('express');
const cors = require('cors');
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const multer = require('multer');
const path = require('path');

const app = express();
const PORT = process.env.PORT || 5000;

// Middleware
app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// MongoDB connection (replace with your MongoDB URI)
mongoose.connect(mongoose.connect('mongodb+srv://shabanavvr:<db_password>@cluster0.cgp4axb.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
.then(() => console.log('MongoDB connected'))
.catch(err => console.log('MongoDB connection error:', err));

// File storage configuration
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + path.extname(file.originalname));
  }
});

const upload = multer({ storage });

// Define Schemas
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  role: { type: String, enum: ['student', 'recruiter'], required: true },
  createdAt: { type: Date, default: Date.now }
});

const studentSchema = new mongoose.Schema({
  userId: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  college: { type: String, required: true },
  skills: [String],
  bio: String,
  resumeUrl: String,
  collegeType: { type: String, enum: ['tier1', 'tier2', 'tier3'], default: 'tier3' }
});

const recruiterSchema = new mongoose.Schema({
  userId: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  company: { type: String, required: true },
  industry: String,
  companySize: String,
  description: String
});

const jobSchema = new mongoose.Schema({
  recruiterId: { type: mongoose.Schema.Types.ObjectId, ref: 'Recruiter', required: true },
  title: { type: String, required: true },
  description: { type: String, required: true },
  skills: [String],
  location: String,
  remote: Boolean,
  postedAt: { type: Date, default: Date.now }
});

// Create models
const User = mongoose.model('User', userSchema);
const Student = mongoose.model('Student', studentSchema);
const Recruiter = mongoose.model('Recruiter', recruiterSchema);
const Job = mongoose.model('Job', jobSchema);

// Simple AI functions for analysis
const analyzeResume = (resumeText) => {
  // In a real application, this would use NLP or ML models
  // Here's a simplified version that looks for keywords
  
  const skills = [];
  const keywords = [
    'javascript', 'python', 'java', 'react', 'node', 'angular', 'vue', 
    'mongodb', 'sql', 'aws', 'docker', 'kubernetes', 'machine learning',
    'data science', 'html', 'css', 'express'
  ];
  
  keywords.forEach(keyword => {
    if (resumeText.toLowerCase().includes(keyword)) {
      skills.push(keyword);
    }
  });
  
  const strengths = [];
  if (skills.length > 5) strengths.push('Diverse skill set');
  if (resumeText.includes('project') || resumeText.includes('projects')) {
    strengths.push('Project experience');
  }
  if (resumeText.includes('team') || resumeText.includes('collaboration')) {
    strengths.push('Teamwork');
  }
  
  const improvements = [];
  if (!resumeText.includes('certification') && !resumeText.includes('certified')) {
    improvements.push('Add relevant certifications');
  }
  if (!resumeText.includes('achievement') && !resumeText.includes('award')) {
    improvements.push('Highlight key achievements');
  }
  
  return {
    skills,
    strengths,
    improvements,
    industryMatch: Math.floor(Math.random() * 30) + 70 // Simulating a match percentage
  };
};

const matchJobWithCandidate = (jobDescription, candidates) => {
  // Simple matching algorithm based on skill overlap
  const jobSkills = [];
  const keywords = [
    'javascript', 'python', 'java', 'react', 'node', 'angular', 'vue', 
    'mongodb', 'sql', 'aws', 'docker', 'kubernetes', 'machine learning',
    'data science', 'html', 'css', 'express'
  ];
  
  keywords.forEach(keyword => {
    if (jobDescription.toLowerCase().includes(keyword)) {
      jobSkills.push(keyword);
    }
  });
  
  const matches = candidates.map(candidate => {
    const sharedSkills = candidate.skills.filter(skill => 
      jobSkills.includes(skill.toLowerCase())
    );
    
    const matchScore = (sharedSkills.length / jobSkills.length) * 100;
    
    return {
      ...candidate._doc,
      matchScore: Math.round(matchScore),
      matchedSkills: sharedSkills
    };
  });
  
  return matches.sort((a, b) => b.matchScore - a.matchScore);
};

// Authentication Middleware
const authenticate = (req, res, next) => {
  try {
    const token = req.headers.authorization.split(' ')[1];
    const decoded = jwt.verify(token, 'your_jwt_secret');
    req.userData = decoded;
    next();
  } catch (error) {
    return res.status(401).json({ message: 'Authentication failed' });
  }
};

// Routes
// Register user
app.post('/api/register', async (req, res) => {
  try {
    const { name, email, password, role, extraData } = req.body;
    
    // Check if user already exists
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(409).json({ message: 'User already exists' });
    }
    
    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);
    
    // Create user
    const user = new User({
      name,
      email,
      password: hashedPassword,
      role
    });
    
    const savedUser = await user.save();
    
    // Create profile based on role
    if (role === 'student') {
      const student = new Student({
        userId: savedUser._id,
        college: extraData.college,
        skills: extraData.skills || [],
        bio: extraData.bio || '',
        collegeType: extraData.collegeType || 'tier3'
      });
      
      await student.save();
    } else if (role === 'recruiter') {
      const recruiter = new Recruiter({
        userId: savedUser._id,
        company: extraData.company,
        industry: extraData.industry || '',
        companySize: extraData.companySize || '',
        description: extraData.description || ''
      });
      
      await recruiter.save();
    }
    
    res.status(201).json({ message: 'User created successfully' });
  } catch (error) {
    res.status(500).json({ message: 'Server error', error: error.message });
  }
});

// Login
app.post('/api/login', async (req, res) => {
  try {
    const { email, password } = req.body;
    
    // Find user
    const user = await User.findOne({ email });
    if (!user) {
      return res.status(401).json({ message: 'Auth failed' });
    }
    
    // Check password
    const passwordMatch = await bcrypt.compare(password, user.password);
    if (!passwordMatch) {
      return res.status(401).json({ message: 'Auth failed' });
    }
    
    // Generate JWT
    const token = jwt.sign(
      { userId: user._id, email: user.email, role: user.role },
      'your_jwt_secret',
      { expiresIn: '1h' }
    );
    
    // Get profile data
    let profileData = {};
    if (user.role === 'student') {
      profileData = await Student.findOne({ userId: user._id });
    } else if (user.role === 'recruiter') {
      profileData = await Recruiter.findOne({ userId: user._id });
    }
    
    res.status(200).json({
      message: 'Auth successful',
      token,
      userId: user._id,
      role: user.role,
      name: user.name,
      profileData
    });
  } catch (error) {
    res.status(500).json({ message: 'Server error', error: error.message });
  }
});

// Upload and analyze resume
app.post('/api/resume/analyze', authenticate, upload.single('resume'), async (req, res) => {
  try {
    if (req.userData.role !== 'student') {
      return res.status(403).json({ message: 'Not authorized' });
    }
    
    // In a real app, we would extract text from the resume file
    // For demo, we'll use the text from the request body
    const resumeText = req.body.resumeText || 'Default resume text with javascript react nodejs';
    
    // Analyze resume
    const analysis = analyzeResume(resumeText);
    
    // Update student profile with resume URL and extracted skills
    if (req.file) {
      await Student.findOneAndUpdate(
        { userId: req.userData.userId },
        { 
          resumeUrl: `/uploads/${req.file.filename}`,
          $addToSet: { skills: { $each: analysis.skills } }
        }
      );
    }
    
    res.status(200).json({ analysis });
  } catch (error) {
    res.status(500).json({ message: 'Server error', error: error.message });
  }
});

// Analyze job description
app.post('/api/job/analyze', authenticate, async (req, res) => {
  try {
    const { jobDescription } = req.body;
    
    // In a real app, we would use NLP to extract requirements
    // Here's a simplified version
    const analysis = analyzeResume(jobDescription); // Reusing the function for simplicity
    
    // If student is requesting this, compare with their profile
    if (req.userData.role === 'student') {
      const student = await Student.findOne({ userId: req.userData.userId });
      
      const studentSkills = student.skills.map(s => s.toLowerCase());
      const jobSkills = analysis.skills.map(s => s.toLowerCase());
      
      const matchedSkills = studentSkills.filter(skill => jobSkills.includes(skill));
      const missingSkills = jobSkills.filter(skill => !studentSkills.includes(skill));
      
      const matchPercentage = jobSkills.length > 0 
        ? Math.round((matchedSkills.length / jobSkills.length) * 100) 
        : 0;
      
      res.status(200).json({
        analysis,
        studentMatch: {
          matchPercentage,
          matchedSkills,
          missingSkills
        }
      });
    } else {
      res.status(200).json({ analysis });
    }
  } catch (error) {
    res.status(500).json({ message: 'Server error', error: error.message });
  }
});

// Post a job
app.post('/api/jobs', authenticate, async (req, res) => {
  try {
    if (req.userData.role !== 'recruiter') {
      return res.status(403).json({ message: 'Not authorized' });
    }
    
    const recruiter = await Recruiter.findOne({ userId: req.userData.userId });
    if (!recruiter) {
      return res.status(404).json({ message: 'Recruiter profile not found' });
    }
    
    const { title, description, skills, location, remote } = req.body;
    
    const job = new Job({
      recruiterId: recruiter._id,
      title,
      description,
      skills: skills || [],
      location,
      remote: remote || false
    });
    
    await job.save();
    
    res.status(201).json({ message: 'Job posted successfully', jobId: job._id });
  } catch (error) {
    res.status(500).json({ message: 'Server error', error: error.message });
  }
});

// Find matching candidates
app.post('/api/candidates/match', authenticate, async (req, res) => {
  try {
    if (req.userData.role !== 'recruiter') {
      return res.status(403).json({ message: 'Not authorized' });
    }
    
    const { jobDescription } = req.body;
    
    // Get all students
    const students = await Student.find().populate('userId', 'name email');
    
    // Match with job description
    const matches = matchJobWithCandidate(jobDescription, students);
    
    // Return top matches
    res.status(200).json({ 
      matches: matches.slice(0, 10).map(match => ({
        id: match._id,
        name: match.userId.name,
        college: match.college,
        collegeType: match.collegeType,
        skills: match.skills,
        matchScore: match.matchScore,
        matchedSkills: match.matchedSkills
      }))
    });
  } catch (error) {
    res.status(500).json({ message: 'Server error', error: error.message });
  }
});

// Get recruiter's posted jobs
app.get('/api/recruiter/jobs', authenticate, async (req, res) => {
  try {
    if (req.userData.role !== 'recruiter') {
      return res.status(403).json({ message: 'Not authorized' });
    }
    
    const recruiter = await Recruiter.findOne({ userId: req.userData.userId });
    if (!recruiter) {
      return res.status(404).json({ message: 'Recruiter profile not found' });
    }
    
    const jobs = await Job.find({ recruiterId: recruiter._id }).sort({ postedAt: -1 });
    
    res.status(200).json({ jobs });
  } catch (error) {
    res.status(500).json({ message: 'Server error', error: error.message });
  }
});

// Update profile
app.put('/api/profile', authenticate, async (req, res) => {
  try {
    const { role } = req.userData;
    const { profileData } = req.body;
    
    if (role === 'student') {
      await Student.findOneAndUpdate(
        { userId: req.userData.userId },
        { 
          college: profileData.college,
          skills: profileData.skills,
          bio: profileData.bio
        }
      );
    } else if (role === 'recruiter') {
      await Recruiter.findOneAndUpdate(
        { userId: req.userData.userId },
        { 
          company: profileData.company,
          industry: profileData.industry,
          companySize: profileData.companySize,
          description: profileData.description
        }
      );
    }
    
    res.status(200).json({ message: 'Profile updated successfully' });
  } catch (error) {
    res.status(500).json({ message: 'Server error', error: error.message });
  }
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
