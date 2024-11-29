import streamlit as st
import pandas as pd
import numpy as np

def calculate_rpn(row):
    return row['Severity'] * row['Occurrence'] * row['Detection']

def main():
    st.title("FMEA (Failure Modes and Effects Analysis)")
    st.write("Failure Modes and Effects Analysis (FMEA) is a structured approach to identify, prioritize, and mitigate risks in a process, product, or system.")

    st.sidebar.header("Navigation")
    pages = ["Introduction", "Create FMEA Table", "Analyze Risks"]
    choice = st.sidebar.radio("Go to", pages)

    if choice == "Introduction":
        st.header("What is FMEA?")
        st.write(
            """FMEA is a systematic method for identifying and addressing potential failure modes in a process or product. It is commonly used in manufacturing, engineering, and other industries to improve reliability and safety.\n\nKey Components of FMEA:\n- **Failure Mode**: Ways in which a process or product might fail.\n- **Effect**: The consequence of the failure mode.\n- **Cause**: The reason why the failure mode occurs.\n- **Severity (S)**: How serious the effect of the failure is, rated on a scale (e.g., 1-10).\n- **Occurrence (O)**: Likelihood of the failure happening, rated on a scale (e.g., 1-10).\n- **Detection (D)**: Likelihood of detecting the failure before it occurs, rated on a scale (e.g., 1-10).\n\nThe Risk Priority Number (RPN) is calculated as: \n\n**RPN = Severity × Occurrence × Detection**\n            """)

    elif choice == "Create FMEA Table":
        st.header("Create FMEA Table")
        st.write("Input your failure modes, effects, causes, and ratings below.")

        if "fmea_table" not in st.session_state:
            st.session_state.fmea_table = pd.DataFrame(
                columns=["Failure Mode", "Effect", "Cause", "Severity", "Occurrence", "Detection", "RPN"]
            )

        with st.form("fmea_form"):
            failure_mode = st.text_input("Failure Mode")
            effect = st.text_input("Effect")
            cause = st.text_input("Cause")
            severity = st.number_input("Severity (1-10)", min_value=1, max_value=10, step=1)
            occurrence = st.number_input("Occurrence (1-10)", min_value=1, max_value=10, step=1)
            detection = st.number_input("Detection (1-10)", min_value=1, max_value=10, step=1)
            submitted = st.form_submit_button("Add to Table")

            if submitted:
                new_entry = {
                    "Failure Mode": failure_mode,
                    "Effect": effect,
                    "Cause": cause,
                    "Severity": severity,
                    "Occurrence": occurrence,
                    "Detection": detection,
                }
                new_entry["RPN"] = calculate_rpn(new_entry)
                st.session_state.fmea_table = st.session_state.fmea_table.append(new_entry, ignore_index=True)
                st.success("Entry added to the table!")

        st.write("### FMEA Table")
        st.dataframe(st.session_state.fmea_table)

    elif choice == "Analyze Risks":
        st.header("Analyze Risks")
        if "fmea_table" in st.session_state and not st.session_state.fmea_table.empty:
            st.write("### FMEA Table with RPNs")
            st.dataframe(st.session_state.fmea_table)

            threshold = st.slider("Set RPN Threshold for High Risk", min_value=1, max_value=1000, value=100)

            high_risk_items = st.session_state.fmea_table[st.session_state.fmea_table["RPN"] >= threshold]

            st.write("### High-Risk Items")
            if not high_risk_items.empty:
                st.dataframe(high_risk_items)
            else:
                st.write("No high-risk items identified.")

        else:
            st.warning("No data available. Please add entries in the 'Create FMEA Table' section.")

if __name__ == "__main__":
    main()
