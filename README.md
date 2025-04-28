# fortune-chat
占い
# streamlit_chat_fortune.py
import streamlit as st
import openai

# OpenAI APIキーを入力（安全のためは.envファイルにするのが本当は良い）
openai.api_key = st.secrets["OPENAI_API_KEY"]

st.title("占いチャット")

# 占いたいテーマを選択
theme = st.selectbox("占いたいテーマを選んでください", ["恋愛", "仕事", "健康", "未来", "人間関係"])

# 相談内容を入力
question = st.text_input("相談したい内容を入力してください")

if st.button("占う"):
    if question:
        with st.spinner("占い中..."):
            response = openai.ChatCompletion.create(
                model="gpt-4o",
                messages=[
                    {"role": "system", "content": f"あなたは優しくプロフェッショナルな占い師です。{theme}に関する相談に答えてください。"},
                    {"role": "user", "content": question}
                ]
            )
            answer = response.choices[0].message.content
            st.success("占い結果")
            st.write(answer)
    else:
        st.warning("相談内容を入力してください。")
