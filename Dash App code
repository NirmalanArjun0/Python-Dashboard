import dash
from dash import dcc, html
import pandas as pd
import plotly.express as px
from dash.dependencies import Input, Output
# Library for theme and template
import dash_bootstrap_components as dbc 
from dash_bootstrap_templates import load_figure_template


# dataset
data = pd.read_csv("europe_sales_records_2012_2017.csv")
data['Order Date'] = pd.to_datetime(data['Order Date'])

# line Dropdown options
dropdown_options = [{'label': 'Total Revenue', 'value': 'Total Revenue'},
                    {'label': 'Total Cost', 'value': 'Total Cost'},
                    {'label': 'Total Profit', 'value': 'Total Profit'}]


# Initialize the app
app = dash.Dash(__name__,assets_folder="assets",external_stylesheets=[dbc.themes.SLATE])
app.title = "Europe Sales Performence"

load_figure_template('CYBORG')

title = html.Div(
    html.H1( "Europe Sales Performence",
            className='supermarket-title'
        )
    
)


# Add footer
footer = html.Div(
            children=[
                html.P('Designed By: N.ARJUN -005 & C.RAGSHAN -009', className='footer-text'),
            ],
    className='footer-container')

# Tab 1
tab1_layout = html.Div(
    children=[
        html.H2("Europe Sales Records Analysis Dashboard", className='H2'),
        html.P("This dashboard provides insights into the performance and profitability of sales across countries in the Europe region from the period of 2010 to 2017.", className='HP'),
        html.P("The dataset used in this analysis contains records from multiple sales and includes details on order dates, quantities sold, pricing, and profitability.",  className='HP'),

        html.H3("Through this analysis, the company can effectively:", className='H3'),
        html.Ul( className='Hl',
                children=[
                    html.Li("Analyze which item types generate the most revenue in each country,"),
                    html.Li("Analyze the quantity of products sold for each item type across all countries to identify the countries where specific products have high sales volumes,"),
                    html.Li("Determine the country-item combinations with the highest order priority,"),
                    html.Li("Identifying which item types have high priority in each country,"),
                    html.Li("Analyze the total profit compared to the total cost to understand profitability,"),
                    html.Li("Study the trends in total revenue and total profit over the years to identify growth strategies.")
                ])
    ]
)

#
#
# Tab 2
tab2_layout = html.Div(
    children=[
        html.H3(className='Tab_sub_title'),    
        dcc.Dropdown(
            id='Line-variable-dropdown',
            options=dropdown_options,
            value='Total Revenue',
            multi=True,
            style={'width': '420px',  'left':'2%', 'color': 'black', 'margin-top': '30px'},
            className='custom-dropdown'  
        ),

        dcc.Graph(
            id='graphoutput',
            style={'width': '99%', 'height': '50vh', 'position': 'relative', 'bottom': '0px','margin': '1px 5px 30px 5px','margin-top': '25px'}
        ),

        dcc.RangeSlider(
            id='date-slider',
            min=data['Order Date'].min().timestamp(),
            max=data['Order Date'].max().timestamp(),
            marks={int(date.timestamp()): {'label': date.strftime('%Y'), 'style': {'color': '#05F0F8', 'font-weight': 'bold'}} for date in pd.date_range(start=data['Order Date'].min(), end=data['Order Date'].max(), freq='Y')},
            value=[data['Order Date'].min().timestamp(), data['Order Date'].max().timestamp()],
            className='custom-style',
        ),
        html.H3("Select Date Range:", className='dt')
    ],
)




# Tab 3
tab3_layout = html.Div([
    dcc.RadioItems(
        id='variable-radio',
        options=dropdown_options,
        value='Total Revenue',
        labelStyle={'display': 'inline-block', 'margin-right': '18%', 'color': '#32FF92','margin-top': '30px' }, 
        style={'width': '45%','margin-left': '32%'}
    ),
    dcc.Graph(id='scatter-plot', style={'width': '96%', 'height': '60vh', 'margin': 'auto', 'position': 'relative', 'top': '30px'}),
])


# Tab 4

# Defining the graphs
default_itemcount = data.groupby('Item Type')['Units Sold'].sum().reset_index()
defaultbar = px.bar(default_itemcount, x='Item Type', y='Units Sold', title="Units sold by Item",
                            labels={'x': 'Item Type', 'y': 'Units Sold'})

defaultbar.update_traces(marker_color='#5EADFE')


#default map
defaultmap = px.choropleth(data, locations="Country", locationmode="country names",  title="Country-wise Revenue",
#setting up the map                    
                    color="Total Revenue", color_continuous_scale=px.colors.diverging.Picnic, range_color=(0, 6000000)).update_geos(visible=False, resolution=50,
                    showcoastlines=True, showland=True, showframe=False, projection_type='equirectangular', lonaxis_range=[-8, 42], lataxis_range=[35, 70])

defaultmap.update_layout(template="plotly_dark")
defaultbar.update_layout(template="plotly_dark")


tab4_layout = html.Div([
    dcc.Graph(
        id='europe-map',
        style={'position': 'absolute', 'bottom': '6vh', 'left': '0', 'width': '50%', 'height': '70vh'},
        figure=defaultmap
    ),
    dcc.Graph(
        id='country-item-bar',
        figure=defaultbar,
        style={'position': 'absolute', 'bottom': '6vh', 'right': '0', 'width': '50%', 'height': '70vh'}
    ),
])

# Tab 5
tab5_layout = html.Div([
    dcc.Graph(
        id='pie-chart',
        figure=px.pie(data, names='Item Type', values='Units Sold', title='Item-Types', template="plotly_dark").update_traces(textinfo='percent+label', showlegend=False),
        style={'width': '50%', 'height': '80vh', 'float': 'left'}
),
html.H3(className='Tab_sub_title'),
    dcc.Graph(
       id='item-order-priority-bar',
       style={'position': 'absolute', 'top': '1', 'right': '0', 'width': '50%', 'height': '40vh'}
),
html.H3(className='Tab_sub_title'),
    dcc.Graph(
       id='line-chart',
       style={'position': 'absolute', 'top': '58.7%', 'right': '0', 'width': '50%', 'height': '40vh'}
)
], style={'display': 'flex'})




# Defining the tabs
tabs = dcc.Tabs(id='tabs', value='tab-1', children=[
    dcc.Tab(label='INTRODUCTION', value='tab-1', children=tab1_layout,className= 'tab--clicked', style={'backgroundColor': '#5EADFE', 'color': 'black', 'font-size': '13px', 'font-weight': 'bold', 'border-radius': '25px'}),
    dcc.Tab(label='Profits & Trends', value='tab-2', children=tab2_layout, className= 'tab--clicked', style={'backgroundColor': '#5EADFE', 'color': 'black', 'font-size': '13px', 'font-weight': 'bold', 'border-radius': '25px'}),
    dcc.Tab(label='Sales Performe', value='tab-3', children=tab3_layout,className= 'tab--clicked', style={'backgroundColor': '#5EADFE', 'color': 'black', 'font-size': '13px', 'font-weight': 'bold', 'border-radius': '25px'}),
    dcc.Tab(label='Sales by Country', value='tab-4', children=tab4_layout,className= 'tab--clicked', style={'backgroundColor': '#5EADFE', 'color': 'black', 'font-size': '13px', 'font-weight': 'bold', 'border-radius': '25px'}),
    dcc.Tab(label='Priority & Profits', value='tab-5', children=tab5_layout,className= 'tab--clicked', style={'backgroundColor': '#5EADFE', 'color': 'black', 'font-size': '13px', 'font-weight': 'bold', 'border-radius': '25px'}),
], style={'fontFamily': 'Times New Roman',  'padding': '5px', 'width': '60%', 'margin': 'auto', 'backgroundColor': '#1b2431', 'color': '#fe0002', 'border-radius': '15px'})


# Layout of the app
app.layout = html.Div( 
            children=[title,footer,tabs]
        )


# Callback updating line graph Tab 2
@app.callback(
    Output('graphoutput', 'figure'),
    [Input('Line-variable-dropdown', 'value'),
     Input('date-slider', 'value')]
)
def update_graph(selectedvaria, date_range):
    startdate = pd.to_datetime(date_range[0], unit='s')
    enddate = pd.to_datetime(date_range[1], unit='s')
    filteredata = data[(data['Order Date'] >= startdate) & (data['Order Date'] <= enddate)]

    fig = px.line(title=f'Performance Over Time for {selectedvaria}', template="plotly_dark")

#for only select one variable
    if isinstance(selectedvaria, str):  
        selectedvaria = [selectedvaria]

    for variable in selectedvaria:
        groupdata = filteredata.groupby(filteredata['Order Date'].dt.year)[variable].sum().reset_index()
        fig.add_scatter(x=groupdata['Order Date'], y=groupdata[variable], mode='lines', name=variable.capitalize(),
                        line=dict(width=3))

    
    fig.update_xaxes(title_text="Year")
    
# y-axis label for each selected variable accordingly
    for variable in selectedvaria:
        fig.update_yaxes(title_text=variable.capitalize())

    return fig


# Callback updating scatter plot Tab 3
@app.callback(
    Output('scatter-plot','figure'),
    [Input('variable-radio','value')]
)
def update_scatter_plot(selected_variable):

#scatter plot
    fig = px.scatter(data, x='Units Sold', y=selected_variable,
                     title=f'Scatter Plot: Units Sold vs {selected_variable}', template="plotly_dark",
                     color_discrete_sequence=['#FF0D0D'])

#correlation findings
    correlation = data['Units Sold'].corr(data[selected_variable])

 #Update layout
    fig.update_layout(title=f'Scatter Plot: Units Sold vs {selected_variable} (Correlation: {correlation:.2f})')
    fig.update_xaxes(title_text="Unit Sold")
    fig.update_yaxes(title_text=selected_variable.capitalize())

    return fig



# Callback map & bar Tab 4
@app.callback(
    Output('country-item-bar', 'figure'),
    [Input('europe-map', 'clickData')]
)
def update_bar_graph(clickData):
    if clickData is None:
        return defaultbar
    else:
        selectingcountry = clickData['points'][0]['location']
        countrydata = data[data['Country'] == selectingcountry]
        itemcount = countrydata.groupby('Item Type')['Units Sold'].sum().reset_index()

        fig = px.bar(itemcount, x='Item Type', y='Units Sold', title=f'Units sold by Item in {selectingcountry}', template="plotly_dark")
        fig.update_traces(marker_color='#5EADFE')
        fig.update_layout( template="plotly_dark")
    
        return fig
    

# Callback update ie, bar & line in Tab 5
@app.callback(
    [Output('line-chart', 'figure'),
     Output('item-order-priority-bar', 'figure')],
    [Input('pie-chart', 'clickData')]
)
def update_charts(clickData):
    if clickData is None:
        originaldata = data.groupby('Order Date').agg({'Total Revenue': 'sum', 'Total Profit': 'sum'}).reset_index()
        defaultline = px.line(originaldata, x='Order Date', y=['Total Revenue', 'Total Profit'], title="Total Revenue and Profit Over the Period:",
                                   color_discrete_map={'Total Revenue': 'red', 'Total Profit': '#08FC10'})
        
        defaultline.update_layout(legend=dict(x=0.98, y=1.5, xanchor='right', yanchor='top', font=dict(size=10)))
        defaultline.update_xaxes(tickformat='%Y')
        defaultline.update_layout(xaxis_title='Order Date', yaxis_title='Amount')
        defaultline.update_layout( template="plotly_dark")

        defaultprioritycounts = data['Order Priority'].value_counts().reset_index()
        defaultprioritycounts.columns = ['Order Priority', 'Count']

        defaultbar = px.bar(defaultprioritycounts, x='Count', y='Order Priority',
                                 title="Order Priority Distribution for Item Types:", orientation='h')

        defaultbar.update_layout(xaxis_title='Count', yaxis_title='Order Priority')
        defaultbar.update_yaxes(categoryorder='total ascending', tickvals=[0, 1, 2, 3],
                                      ticktext=["C: Critical & High", "H: High", "L: Low", "M: Medium"])
        defaultbar.update_layout( template="plotly_dark")

        return defaultline, defaultbar

    else:
        itemtype = clickData['points'][0]['label']
        filteredata = data[data['Item Type'] == itemtype]
        groupdata = filteredata.groupby(filteredata['Order Date'].dt.year)[['Total Revenue', 'Total Profit']].sum().reset_index()

        linechart = px.line(groupdata, x='Order Date', y=['Total Revenue', 'Total Profit'],
                           title=f'Total Revenue and Profit Over Time for {itemtype}',
                           color_discrete_map={'Total Revenue': 'red', 'Total Profit': '#08FC10'})

        linechart.update_layout(legend=dict(x=0.98, y=1.5, xanchor='right', yanchor='top', font=dict(size=10)))
        linechart.update_xaxes(title_text="Year")
        linechart.update_yaxes(title_text="Amount")
        linechart.update_layout( template="plotly_dark")

        prioritycounts = filteredata['Order Priority'].value_counts().reset_index()
        prioritycounts.columns = ['Order Priority', 'Count']

        barchart = px.bar(prioritycounts, x='Count', y='Order Priority',
                         title=f'Order Priority Distribution for {itemtype}', orientation='h')

        barchart.update_yaxes(title_text="Order Priority")
        barchart.update_xaxes(title_text="Count")
        barchart.update_yaxes(categoryorder='total ascending', tickvals=[0, 1, 2, 3],
                              ticktext=["C: Critical & High", "H: High", "L: Low", "M: Medium"])
        barchart.update_layout( template="plotly_dark")  
        return linechart, barchart


if __name__ == '__main__':
    app.run_server(debug=True)
