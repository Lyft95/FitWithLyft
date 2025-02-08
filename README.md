# FitWithLyft
from fastapi import FastAPI
import openai

app = FastAPI()

openai.api_key = "your-openai-api-key"

@app.get("/generate_app")
def generate_app(description: str):
    prompt = f"Generate Swift code for an iOS fitness app. Features: {description}."
    
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "system", "content": prompt}]
    )
    
    return {"swift_code": response["choices"][0]["message"]["content"]}
import SwiftUI

struct ContentView: View {
    @State private var generatedCode = ""

    var body: some View {
        VStack {
            Text("AI Code Generator")
                .font(.title)
                .padding()
            
            Button(action: fetchGeneratedCode) {
                Text("Generate Fitness App Code")
                    .padding()
                    .background(Color.blue)
                    .foregroundColor(.white)
                    .cornerRadius(10)
            }
            .padding()
            
            ScrollView {
                Text(generatedCode)
                    .padding()
                    .font(.system(.body, design: .monospaced))
            }
        }
    }

    func fetchGeneratedCode() {
        guard let url = URL(string: "https://your-backend-api.com/generate_app?description=Workout tracking, AI coaching, SwiftUI") else { return }
        var request = URLRequest(url: url)
        request.httpMethod = "GET"

        URLSession.shared.dataTask(with: request) { data, _, _ in
            if let data = data {
                if let response = try? JSONDecoder().decode([String: String].self, from: data) {
                    DispatchQueue.main.async {
                        self.generatedCode = response["swift_code"] ?? "No code generated."
                    }
                }
            }
        }.resume()
    }
}