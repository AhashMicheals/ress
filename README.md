# ress
import streamlit as st
from io import BytesIO
from fpdf import FPDF

# Function to generate PDF
def generate_resume(name, contact, email, education, experience, skills):
    pdf = FPDF()
    pdf.set_auto_page_break(auto=True, margin=15)
    pdf.add_page()
    pdf.set_font("Arial", size=12)

    # Header
    pdf.set_font("Arial", style="B", size=16)
    pdf.cell(200, 10, txt="Resume", ln=True, align="C")
    pdf.ln(10)

    # Personal Details
    pdf.set_font("Arial", style="B", size=14)
    pdf.cell(200, 10, txt="Personal Details", ln=True, align="L")
    pdf.set_font("Arial", size=12)
    pdf.cell(200, 10, txt=f"Name: {name}", ln=True)
    pdf.cell(200, 10, txt=f"Contact: {contact}", ln=True)
    pdf.cell(200, 10, txt=f"Email: {email}", ln=True)
    pdf.ln(10)

    # Education
    pdf.set_font("Arial", style="B", size=14)
    pdf.cell(200, 10, txt="Education", ln=True, align="L")
    pdf.set_font("Arial", size=12)
    pdf.multi_cell(0, 10, education)
    pdf.ln(10)

    # Work Experience
    pdf.set_font("Arial", style="B", size=14)
    pdf.cell(200, 10, txt="Work Experience", ln=True, align="L")
    pdf.set_font("Arial", size=12)
    pdf.multi_cell(0, 10, experience)
    pdf.ln(10)

    # Skills
    pdf.set_font("Arial", style="B", size=14)
    pdf.cell(200, 10, txt="Skills", ln=True, align="L")
    pdf.set_font("Arial", size=12)
    pdf.multi_cell(0, 10, skills)

    # Save to BytesIO
    pdf_output = BytesIO()
    pdf.output(pdf_output)
    return pdf_output

# Streamlit UI
st.title("AI Resume Builder")

st.sidebar.title("Input Your Details")
name = st.sidebar.text_input("Full Name")
contact = st.sidebar.text_input("Contact Number")
email = st.sidebar.text_input("Email Address")

st.sidebar.title("Education")
education = st.sidebar.text_area("Add your education details (e.g., degrees, institutions, years):")

st.sidebar.title("Work Experience")
experience = st.sidebar.text_area("Add your work experience details (e.g., roles, companies, years):")

st.sidebar.title("Skills")
skills = st.sidebar.text_area("List your skills (e.g., Python, JavaScript, Data Analysis):")

if st.sidebar.button("Generate Resume"):
    if name and contact and email and education and experience and skills:
        pdf_data = generate_resume(name, contact, email, education, experience, skills)
        st.success("Resume generated successfully!")
        st.download_button(
            label="Download Resume PDF",
            data=pdf_data.getvalue(),
            file_name="resume.pdf",
            mime="application/pdf"
        )
    else:
        st.error("Please fill in all the details.")
