---
name: data-intelligence
description: 数据情报分析引擎。用于对收集的信息进行深度分析、模式识别、趋势预测、数据可视化。支持多源数据融合、自动生成洞察报告、机器学习辅助决策。
metadata:
  emoji: "📊"
  requires:
    bins: ['python3', 'pip', 'jq']
---

## 使用条件

### ✅ 何时使用
- 当用户需要对收集的数据进行深度分析、模式识别、趋势预测、生成情报报告时使用此技能

### ❌ 何时不使用
- 不要用于分析非法获取的数据、隐私敏感信息。

## 命令

# 数据情报分析技能

## 📊 技能说明

**用途**：对收集的数据进行深度分析、模式识别、趋势预测、可视化展示

**核心能力**：
- 数据清洗与预处理
- 统计分析与模式识别
- 时间序列趋势预测
- 情报报告自动生成
- 交互式数据可视化

---

## 🛠️ Python数据分析工具

### 1️⃣ 数据分析与统计

```python
import pandas as pd
import numpy as np
from scipy import stats
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.ensemble import IsolationForest

# 数据加载与清洗
def load_and_clean_data(file_path):
    """加载并清洗数据"""
    df = pd.read_csv(file_path)
    
    # 基本清洗
    df = df.drop_duplicates()
    df = df.fillna(method='ffill')
    
    # 去除异常值
    Q1 = df.quantile(0.25)
    Q3 = df.quantile(0.75)
    IQR = Q3 - Q1
    df = df[~((df < (Q1 - 1.5 * IQR)) | (df > (Q3 + 1.5 * IQR))).any(axis=1)]
    
    return df

# 描述性统计
def descriptive_statistics(df):
    """生成描述性统计报告"""
    stats_report = {
        'shape': df.shape,
        'columns': list(df.columns),
        'dtypes': df.dtypes.to_dict(),
        'summary': df.describe().to_dict(),
        'correlations': df.corr().to_dict()
    }
    return stats_report

# 异常检测
def detect_anomalies(df, contamination=0.1):
    """使用Isolation Forest检测异常"""
    scaler = StandardScaler()
    df_scaled = scaler.fit_transform(df.select_dtypes(include=[np.number]))
    
    clf = IsolationForest(contamination=contamination, random_state=42)
    df['anomaly'] = clf.fit_predict(df_scaled)
    
    anomalies = df[df['anomaly'] == -1]
    return anomalies

# 时间序列分析
def time_series_analysis(df, date_column, value_column):
    """时间序列趋势分析"""
    df[date_column] = pd.to_datetime(df[date_column])
    df = df.set_index(date_column)
    
    # 移动平均
    df['MA7'] = df[value_column].rolling(window=7).mean()
    df['MA30'] = df[value_column].rolling(window=30).mean()
    
    # 趋势
    trend = np.polyfit(range(len(df)), df[value_column], 1)
    
    return {
        'trend_slope': trend[0],
        'current_value': df[value_column].iloc[-1],
        'ma7': df['MA7'].iloc[-1],
        'ma30': df['MA30'].iloc[-1]
    }
```

### 2️⃣ 数据可视化

```python
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
import plotly.graph_objects as go

# 趋势图
def plot_trend(df, x_column, y_column, title="趋势分析"):
    """生成交互式趋势图"""
    fig = px.line(df, x=x_column, y=y_column, title=title)
    fig.update_layout(
        hovermode='x unified',
        template='plotly_dark'
    )
    return fig.to_html()

# 热力图
def plot_correlation_heatmap(df):
    """生成相关性热力图"""
    corr = df.corr()
    fig = px.imshow(corr, 
                    text_auto=True, 
                    aspect="auto",
                    color_continuous_scale='RdBu_r')
    return fig.to_html()

# 散点矩阵
def plot_scatter_matrix(df, columns):
    """生成散点矩阵图"""
    fig = px.scatter_matrix(df, dimensions=columns)
    return fig.to_html()

# 仪表板
def create_dashboard(df):
    """创建综合分析仪表板"""
    from plotly.subplots import make_subplots
    
    fig = make_subplots(
        rows=2, cols=2,
        subplot_titles=('趋势分析', '分布分析', '相关性', '异常值'),
        specs=[[{"secondary_y": True}, {"type": "histogram"}],
               [{"type": "heatmap"}, {"type": "scatter"}]]
    )
    
    # 添加各个子图
    # ... 根据数据添加不同类型的图表
    
    fig.update_layout(
        title_text="数据情报分析仪表板",
        showlegend=True,
        height=800
    )
    
    return fig.to_html()
```

### 3️⃣ 自然语言处理

```python
import jieba
from wordcloud import WordCloud
from textblob import TextBlob
from collections import Counter
import re

# 文本清洗
def clean_text(text):
    """清洗文本数据"""
    # 去除特殊字符
    text = re.sub(r'[^\w\s]', '', text)
    # 转小写
    text = text.lower()
    # 分词（中文）
    words = jieba.cut(text)
    return ' '.join(words)

# 情感分析
def sentiment_analysis(text):
    """分析文本情感倾向"""
    blob = TextBlob(text)
    polarity = blob.sentiment.polarity
    subjectivity = blob.sentiment.subjectivity
    
    if polarity > 0.1:
        sentiment = "positive"
    elif polarity < -0.1:
        sentiment = "negative"
    else:
        sentiment = "neutral"
    
    return {
        'sentiment': sentiment,
        'polarity': polarity,
        'subjectivity': subjectivity
    }

# 关键词提取
def extract_keywords(text, top_n=10):
    """提取关键词"""
    words = jieba.cut(text)
    # 过滤停用词
    stopwords = set(['的', '了', '是', '在', '和', '有', '我', '他', '她'])
    keywords = [w for w in words if w not in stopwords and len(w) > 1]
    
    # 统计词频
    word_counts = Counter(keywords)
    return word_counts.most_common(top_n)

# 词云生成
def generate_wordcloud(text, output_path="wordcloud.png"):
    """生成词云"""
    wordcloud = WordCloud(
        font_path='simhei.ttf',  # 中文字体
        width=800,
        height=400,
        background_color='white'
    ).generate(text)
    
    plt.figure(figsize=(10, 5))
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis('off')
    plt.savefig(output_path, dpi=300, bbox_inches='tight')
    plt.close()
    
    return output_path
```

### 4️⃣ 网络分析

```python
import networkx as nx
from community import community_louvain

# 构建网络图
def build_network(nodes, edges):
    """构建网络分析图"""
    G = nx.Graph()
    G.add_nodes_from(nodes)
    G.add_edges_from(edges)
    
    return G

# 社区检测
def detect_communities(G):
    """检测网络社区"""
    partition = community_louvain.best_partition(G)
    return partition

# 关键节点识别
def identify_key_nodes(G, top_n=10):
    """识别关键节点"""
    # 中心性指标
    degree_centrality = nx.degree_centrality(G)
    betweenness_centrality = nx.betweenness_centrality(G)
    pagerank = nx.pagerank(G)
    
    # 综合排序
    key_nodes = []
    for node in G.nodes():
        score = (
            degree_centrality[node] * 0.3 +
            betweenness_centrality[node] * 0.4 +
            pagerank[node] * 0.3
        )
        key_nodes.append((node, score))
    
    return sorted(key_nodes, key=lambda x: x[1], reverse=True)[:top_n]

# 网络可视化
def visualize_network(G, partition=None):
    """可视化网络"""
    import matplotlib.pyplot as plt
    
    plt.figure(figsize=(12, 8))
    
    pos = nx.spring_layout(G)
    
    # 绘制节点
    if partition:
        nx.draw_networkx_nodes(G, pos, 
                              node_color=list(partition.values()),
                              node_size=300,
                              cmap=plt.cm.RdYlBu)
    else:
        nx.draw_networkx_nodes(G, pos, node_size=300)
    
    # 绘制边
    nx.draw_networkx_edges(G, pos, alpha=0.5)
    
    # 绘制标签
    nx.draw_networkx_labels(G, pos, font_size=8)
    
    plt.axis('off')
    plt.savefig("network_analysis.png", dpi=300, bbox_inches='tight')
    plt.close()
    
    return "network_analysis.png"
```

### 5️⃣ 情报报告生成

```python
from datetime import datetime
import json

def generate_intelligence_report(data, analysis_results):
    """生成情报分析报告"""
    report = {
        'metadata': {
            'generated_at': datetime.now().isoformat(),
            'analyst': 'JARVIS-AI',
            'version': '2.0'
        },
        'summary': {
            'total_records': len(data),
            'time_range': f"{data['date'].min()} to {data['date'].max()}",
            'key_findings': []
        },
        'data_overview': {
            'statistics': analysis_results['statistics'],
            'anomalies': analysis_results['anomalies'],
            'trends': analysis_results['trends']
        },
        'analysis': {
            'patterns': analysis_results['patterns'],
            'correlations': analysis_results['correlations'],
            'predictions': analysis_results['predictions']
        },
        'recommendations': [],
        'confidence_level': 'high'
    }
    
    return json.dumps(report, indent=2, ensure_ascii=False)

# 导出为HTML报告
def export_html_report(report_data, output_path="intelligence_report.html"):
    """导出HTML格式报告"""
    html_template = f"""
    <!DOCTYPE html>
    <html>
    <head>
        <title>情报分析报告</title>
        <style>
            body {{ font-family: Arial, sans-serif; margin: 40px; }}
            .header {{ background: #2c3e50; color: white; padding: 20px; }}
            .section {{ margin: 20px 0; padding: 15px; border-left: 3px solid #3498db; }}
            .metric {{ display: inline-block; margin: 10px; padding: 15px; 
                     background: #ecf0f1; border-radius: 5px; }}
            table {{ border-collapse: collapse; width: 100%; }}
            th, td {{ border: 1px solid #ddd; padding: 8px; text-align: left; }}
            th {{ background-color: #3498db; color: white; }}
        </style>
    </head>
    <body>
        <div class="header">
            <h1>📊 数据情报分析报告</h1>
            <p>生成时间: {report_data['metadata']['generated_at']}</p>
        </div>
        
        <div class="section">
            <h2>📋 执行摘要</h2>
            <div class="metric">总记录数: {report_data['summary']['total_records']}</div>
            <div class="metric">时间范围: {report_data['summary']['time_range']}</div>
        </div>
        
        <div class="section">
            <h2>📈 数据概览</h2>
            {report_data['data_overview']}
        </div>
        
        <div class="section">
            <h2>🔍 深度分析</h2>
            {report_data['analysis']}
        </div>
        
        <div class="section">
            <h2>💡 建议</h2>
            <ul>
                {"".join([f"<li>{rec}</li>" for rec in report_data['recommendations']])}
            </ul>
        </div>
    </body>
    </html>
    """
    
    with open(output_path, 'w', encoding='utf-8') as f:
        f.write(html_template)
    
    return output_path
```

---

## 🎯 使用场景

### ✅ 适合使用

- 金融数据分析
- 市场趋势预测
- 用户行为分析
- 网络关系分析
- 文本情报分析
- 异常检测

### ❌ 不适合使用

- 违法数据获取
- 隐私侵犯分析

---

## 📚 快速开始

```bash
# 安装依赖
pip install pandas numpy scipy scikit-learn matplotlib seaborn plotly jieba wordcloud networkx python-louvain

# 运行分析
python data_intelligence_analyzer.py --input data.csv --output report.html
```

---

## ⚠️ 注意事项

- 确保数据来源合法合规
- 注意保护敏感信息
- 分析结果仅供参考
- 建议结合人工判断

---
*此技能由 JARVIS 自动创建*
