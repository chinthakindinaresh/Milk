import streamlit as st
import pandas as pd
from datetime import datetime

DATA_FILE = "yadagiri_milk_point.csv"

# Initialize or load data
def load_data():
    try:
        return pd.read_csv(DATA_FILE)
    except FileNotFoundError:
        return pd.DataFrame(columns=["Date", "Name", "Milk Type", "Rate", "Quantity", "Fat", "Last Visited"])

def save_data(df):
    df.to_csv(DATA_FILE, index=False)

# Streamlit UI
st.title("🥛 Yadagiri Milk Point")

# Form inputs
with st.form("milk_form"):
    date = st.date_input("Date", datetime.now())
    name = st.text_input("Customer Name")
    milk_type = st.selectbox("Milk Type", ["Cow", "Buffalo", "Mixed"])
    rate = st.number_input("Per Litre Rate (₹)", min_value=0.0)
    quantity = st.number_input("Quantity (litres)", min_value=0.0)
    fat = st.number_input("Fat %", min_value=0.0, max_value=10.0)
    last_visited = st.date_input("Last Visited Date")

    submitted = st.form_submit_button("Add Record")

    if submitted:
        df = load_data()
        new_record = {
            "Date": date,
            "Name": name,
            "Milk Type": milk_type,
            "Rate": rate,
            "Quantity": quantity,
            "Fat": fat,
            "Last Visited": last_visited
        }
        df = pd.concat([df, pd.DataFrame([new_record])], ignore_index=True)
        save_data(df)
        st.success("✅ Record added successfully!")

# Show records
if st.checkbox("📋 Show All Records"):
    df = load_data()
    st.dataframe(df)# Milk
