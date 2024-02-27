import websocket
import json
import ssl
import plotly.graph_objs as go
from IPython.display import IFrame

# Initialize a Plotly chart
fig = go.FigureWidget(layout={'title': 'SPY Minute Aggregates'})
fig.add_scatter()


def on_message(ws, message):
    # Parse the incoming message
    data = json.loads(message)

    # Assume the message has 't' for timestamp and 'o' for open price
    timestamp = data['t']
    open_price = data['o']

    # Update the Plotly chart with the new data
    fig.data[0].x = fig.data[0].x + (timestamp,)
    fig.data[0].y = fig.data[0].y + (open_price,)

    # Redraw the chart to reflect the updated data
    fig.update_layout(title='SPY Minute Aggregates', xaxis_rangeslider_visible=True)
    fig.show()



import websocket
import json
import ssl
import plotly.graph_objs as go
from IPython.display import display, clear_output


def on_message(ws, message):
    print("Received message:", message)
    # Here, you would add your logic to handle the message
    # For example, parsing the JSON and updating your 1-minute chart data

def on_error(ws, error):
    print("Error:", error)

def on_close(ws, close_status_code, close_msg):
    print("Connection closed")

def on_open(ws):
    print("Connection opened")
    # Authenticate
    ws.send(json.dumps({
        "action": "auth",
        "params": "EcqFgP0PhDpUCcxY6EuDncsAC4_OvXeA"
    }))
    # Subscribe to SPY minute aggregates
    ws.send(json.dumps({
        "action": "subscribe",
        "params": "AM.SPY"
    }))

# Replace 'YOUR_API_KEY' with your actual Polygon.io API key
api_key =

# Polygon.io WebSocket URL
polygon_ws_url = "wss://socket.polygon.io/stocks"

# Create a WebSocket connection to the specified URL
ws = websocket.WebSocketApp(polygon_ws_url,
                            on_open=on_open,
                            on_message=on_message,
                            on_error=on_error,
                            on_close=on_close)

# Run the WebSocket connection with SSL context
ws.run_forever(sslopt={"cert_reqs": ssl.CERT_NONE})

import csv
import datetime
import os

# Initialize a counter for the file names and a timestamp for the minute
file_counter = 1
current_minute = datetime.datetime.now().minute


# This function writes data to a CSV file and creates a new one each minute
def write_data_to_csv(new_data):
    global file_counter
    global current_minute
    now = datetime.datetime.now()

    # Check if the current minute is different from the last data point's minute
    if now.minute != current_minute:
        current_minute = now.minute
        file_counter += 1

    # Define the file name based on the counter
    file_name = f'data{file_counter}.csv'

    # Check if the file already exists; if not, write headers
    file_exists = os.path.isfile(file_name)

    # Open the file in append mode
    with open(file_name, mode='a', newline='') as file:
        writer = csv.writer(file)

        # Write headers if the file is being created
        if not file_exists:
            writer.writerow(['timestamp', 'open', 'high', 'low', 'close'])

        # Write the data
        writer.writerow([new_data['timestamp'], new_data['open'], new_data['high'], new_data['low'], new_data['close']])


# Your existing WebSocket on_message callback
def on_message(ws, message):
    # Parse the incoming message to a dictionary (assuming JSON format)
    new_data = parse_message_to_dict(message)

    # Write the new data to CSV
    write_data_to_csv(new_data)

    def on_error(ws, error):
        print("Error:", error)

    def on_close(ws, close_status_code, close_msg):
        print("Connection closed")

    def on_open(ws):
        print("Connection opened")
        # Authenticate
        ws.send(json.dumps({
            "action": "auth",
            "params": 
        }))
        # Subscribe to SPY minute aggregates
        ws.send(json.dumps({
            "action": "subscribe",
            "params": "AM.SPY"
        }))

    # Replace 'YOUR_API_KEY' with your actual Polygon.io API key
    api_key = 

    # Polygon.io WebSocket URL
    polygon_ws_url = "wss://socket.polygon.io/stocks"

    # Create a WebSocket connection to the specified URL
    ws = websocket.WebSocketApp(polygon_ws_url,
                                on_open=on_open,
                                on_message=on_message,
                                on_error=on_error,
                                on_close=on_close)

    # Run the WebSocket connection with SSL context
    ws.run_forever(sslopt={"cert_reqs": ssl.CERT_NONE})




