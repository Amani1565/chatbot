{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "code",
      "source": [
        "!pip install -U langgraph langsmith\n",
        "!pip install -U langchain-anthropic"
      ],
      "metadata": {
        "id": "pavw2JnWkqwh",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "6644b17b-4fc3-48fc-9fb6-f5e961e629db"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Collecting langgraph\n",
            "  Downloading langgraph-0.4.5-py3-none-any.whl.metadata (7.3 kB)\n",
            "Requirement already satisfied: langsmith in /usr/local/lib/python3.11/dist-packages (0.3.42)\n",
            "Requirement already satisfied: langchain-core>=0.1 in /usr/local/lib/python3.11/dist-packages (from langgraph) (0.3.59)\n",
            "Collecting langgraph-checkpoint<3.0.0,>=2.0.26 (from langgraph)\n",
            "  Downloading langgraph_checkpoint-2.0.26-py3-none-any.whl.metadata (4.6 kB)\n",
            "Collecting langgraph-prebuilt>=0.1.8 (from langgraph)\n",
            "  Downloading langgraph_prebuilt-0.1.8-py3-none-any.whl.metadata (5.0 kB)\n",
            "Collecting langgraph-sdk>=0.1.42 (from langgraph)\n",
            "  Downloading langgraph_sdk-0.1.70-py3-none-any.whl.metadata (1.5 kB)\n",
            "Requirement already satisfied: pydantic>=2.7.4 in /usr/local/lib/python3.11/dist-packages (from langgraph) (2.11.4)\n",
            "Requirement already satisfied: xxhash<4.0.0,>=3.5.0 in /usr/local/lib/python3.11/dist-packages (from langgraph) (3.5.0)\n",
            "Requirement already satisfied: httpx<1,>=0.23.0 in /usr/local/lib/python3.11/dist-packages (from langsmith) (0.28.1)\n",
            "Requirement already satisfied: orjson<4.0.0,>=3.9.14 in /usr/local/lib/python3.11/dist-packages (from langsmith) (3.10.18)\n",
            "Requirement already satisfied: packaging>=23.2 in /usr/local/lib/python3.11/dist-packages (from langsmith) (24.2)\n",
            "Requirement already satisfied: requests<3,>=2 in /usr/local/lib/python3.11/dist-packages (from langsmith) (2.32.3)\n",
            "Requirement already satisfied: requests-toolbelt<2.0.0,>=1.0.0 in /usr/local/lib/python3.11/dist-packages (from langsmith) (1.0.0)\n",
            "Requirement already satisfied: zstandard<0.24.0,>=0.23.0 in /usr/local/lib/python3.11/dist-packages (from langsmith) (0.23.0)\n",
            "Requirement already satisfied: anyio in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.23.0->langsmith) (4.9.0)\n",
            "Requirement already satisfied: certifi in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.23.0->langsmith) (2025.4.26)\n",
            "Requirement already satisfied: httpcore==1.* in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.23.0->langsmith) (1.0.9)\n",
            "Requirement already satisfied: idna in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.23.0->langsmith) (3.10)\n",
            "Requirement already satisfied: h11>=0.16 in /usr/local/lib/python3.11/dist-packages (from httpcore==1.*->httpx<1,>=0.23.0->langsmith) (0.16.0)\n",
            "Requirement already satisfied: tenacity!=8.4.0,<10.0.0,>=8.1.0 in /usr/local/lib/python3.11/dist-packages (from langchain-core>=0.1->langgraph) (9.1.2)\n",
            "Requirement already satisfied: jsonpatch<2.0,>=1.33 in /usr/local/lib/python3.11/dist-packages (from langchain-core>=0.1->langgraph) (1.33)\n",
            "Requirement already satisfied: PyYAML>=5.3 in /usr/local/lib/python3.11/dist-packages (from langchain-core>=0.1->langgraph) (6.0.2)\n",
            "Requirement already satisfied: typing-extensions>=4.7 in /usr/local/lib/python3.11/dist-packages (from langchain-core>=0.1->langgraph) (4.13.2)\n",
            "Collecting ormsgpack<2.0.0,>=1.8.0 (from langgraph-checkpoint<3.0.0,>=2.0.26->langgraph)\n",
            "  Downloading ormsgpack-1.9.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (43 kB)\n",
            "\u001b[2K     \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m43.5/43.5 kB\u001b[0m \u001b[31m1.5 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hRequirement already satisfied: annotated-types>=0.6.0 in /usr/local/lib/python3.11/dist-packages (from pydantic>=2.7.4->langgraph) (0.7.0)\n",
            "Requirement already satisfied: pydantic-core==2.33.2 in /usr/local/lib/python3.11/dist-packages (from pydantic>=2.7.4->langgraph) (2.33.2)\n",
            "Requirement already satisfied: typing-inspection>=0.4.0 in /usr/local/lib/python3.11/dist-packages (from pydantic>=2.7.4->langgraph) (0.4.0)\n",
            "Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.11/dist-packages (from requests<3,>=2->langsmith) (3.4.2)\n",
            "Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.11/dist-packages (from requests<3,>=2->langsmith) (2.4.0)\n",
            "Requirement already satisfied: jsonpointer>=1.9 in /usr/local/lib/python3.11/dist-packages (from jsonpatch<2.0,>=1.33->langchain-core>=0.1->langgraph) (3.0.0)\n",
            "Requirement already satisfied: sniffio>=1.1 in /usr/local/lib/python3.11/dist-packages (from anyio->httpx<1,>=0.23.0->langsmith) (1.3.1)\n",
            "Downloading langgraph-0.4.5-py3-none-any.whl (155 kB)\n",
            "\u001b[2K   \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m155.3/155.3 kB\u001b[0m \u001b[31m3.0 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hDownloading langgraph_checkpoint-2.0.26-py3-none-any.whl (44 kB)\n",
            "\u001b[2K   \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m44.2/44.2 kB\u001b[0m \u001b[31m2.2 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hDownloading langgraph_prebuilt-0.1.8-py3-none-any.whl (25 kB)\n",
            "Downloading langgraph_sdk-0.1.70-py3-none-any.whl (49 kB)\n",
            "\u001b[2K   \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m50.0/50.0 kB\u001b[0m \u001b[31m3.5 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hDownloading ormsgpack-1.9.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (223 kB)\n",
            "\u001b[2K   \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m223.6/223.6 kB\u001b[0m \u001b[31m10.8 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hInstalling collected packages: ormsgpack, langgraph-sdk, langgraph-checkpoint, langgraph-prebuilt, langgraph\n",
            "Successfully installed langgraph-0.4.5 langgraph-checkpoint-2.0.26 langgraph-prebuilt-0.1.8 langgraph-sdk-0.1.70 ormsgpack-1.9.1\n",
            "Collecting langchain-anthropic\n",
            "  Downloading langchain_anthropic-0.3.13-py3-none-any.whl.metadata (1.9 kB)\n",
            "Collecting anthropic<1,>=0.51.0 (from langchain-anthropic)\n",
            "  Downloading anthropic-0.51.0-py3-none-any.whl.metadata (25 kB)\n",
            "Requirement already satisfied: langchain-core<1.0.0,>=0.3.59 in /usr/local/lib/python3.11/dist-packages (from langchain-anthropic) (0.3.59)\n",
            "Requirement already satisfied: pydantic<3.0.0,>=2.7.4 in /usr/local/lib/python3.11/dist-packages (from langchain-anthropic) (2.11.4)\n",
            "Requirement already satisfied: anyio<5,>=3.5.0 in /usr/local/lib/python3.11/dist-packages (from anthropic<1,>=0.51.0->langchain-anthropic) (4.9.0)\n",
            "Requirement already satisfied: distro<2,>=1.7.0 in /usr/local/lib/python3.11/dist-packages (from anthropic<1,>=0.51.0->langchain-anthropic) (1.9.0)\n",
            "Requirement already satisfied: httpx<1,>=0.25.0 in /usr/local/lib/python3.11/dist-packages (from anthropic<1,>=0.51.0->langchain-anthropic) (0.28.1)\n",
            "Requirement already satisfied: jiter<1,>=0.4.0 in /usr/local/lib/python3.11/dist-packages (from anthropic<1,>=0.51.0->langchain-anthropic) (0.9.0)\n",
            "Requirement already satisfied: sniffio in /usr/local/lib/python3.11/dist-packages (from anthropic<1,>=0.51.0->langchain-anthropic) (1.3.1)\n",
            "Requirement already satisfied: typing-extensions<5,>=4.10 in /usr/local/lib/python3.11/dist-packages (from anthropic<1,>=0.51.0->langchain-anthropic) (4.13.2)\n",
            "Requirement already satisfied: langsmith<0.4,>=0.1.125 in /usr/local/lib/python3.11/dist-packages (from langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (0.3.42)\n",
            "Requirement already satisfied: tenacity!=8.4.0,<10.0.0,>=8.1.0 in /usr/local/lib/python3.11/dist-packages (from langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (9.1.2)\n",
            "Requirement already satisfied: jsonpatch<2.0,>=1.33 in /usr/local/lib/python3.11/dist-packages (from langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (1.33)\n",
            "Requirement already satisfied: PyYAML>=5.3 in /usr/local/lib/python3.11/dist-packages (from langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (6.0.2)\n",
            "Requirement already satisfied: packaging<25,>=23.2 in /usr/local/lib/python3.11/dist-packages (from langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (24.2)\n",
            "Requirement already satisfied: annotated-types>=0.6.0 in /usr/local/lib/python3.11/dist-packages (from pydantic<3.0.0,>=2.7.4->langchain-anthropic) (0.7.0)\n",
            "Requirement already satisfied: pydantic-core==2.33.2 in /usr/local/lib/python3.11/dist-packages (from pydantic<3.0.0,>=2.7.4->langchain-anthropic) (2.33.2)\n",
            "Requirement already satisfied: typing-inspection>=0.4.0 in /usr/local/lib/python3.11/dist-packages (from pydantic<3.0.0,>=2.7.4->langchain-anthropic) (0.4.0)\n",
            "Requirement already satisfied: idna>=2.8 in /usr/local/lib/python3.11/dist-packages (from anyio<5,>=3.5.0->anthropic<1,>=0.51.0->langchain-anthropic) (3.10)\n",
            "Requirement already satisfied: certifi in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.25.0->anthropic<1,>=0.51.0->langchain-anthropic) (2025.4.26)\n",
            "Requirement already satisfied: httpcore==1.* in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.25.0->anthropic<1,>=0.51.0->langchain-anthropic) (1.0.9)\n",
            "Requirement already satisfied: h11>=0.16 in /usr/local/lib/python3.11/dist-packages (from httpcore==1.*->httpx<1,>=0.25.0->anthropic<1,>=0.51.0->langchain-anthropic) (0.16.0)\n",
            "Requirement already satisfied: jsonpointer>=1.9 in /usr/local/lib/python3.11/dist-packages (from jsonpatch<2.0,>=1.33->langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (3.0.0)\n",
            "Requirement already satisfied: orjson<4.0.0,>=3.9.14 in /usr/local/lib/python3.11/dist-packages (from langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (3.10.18)\n",
            "Requirement already satisfied: requests<3,>=2 in /usr/local/lib/python3.11/dist-packages (from langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (2.32.3)\n",
            "Requirement already satisfied: requests-toolbelt<2.0.0,>=1.0.0 in /usr/local/lib/python3.11/dist-packages (from langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (1.0.0)\n",
            "Requirement already satisfied: zstandard<0.24.0,>=0.23.0 in /usr/local/lib/python3.11/dist-packages (from langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (0.23.0)\n",
            "Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.11/dist-packages (from requests<3,>=2->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (3.4.2)\n",
            "Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.11/dist-packages (from requests<3,>=2->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.59->langchain-anthropic) (2.4.0)\n",
            "Downloading langchain_anthropic-0.3.13-py3-none-any.whl (26 kB)\n",
            "Downloading anthropic-0.51.0-py3-none-any.whl (263 kB)\n",
            "\u001b[2K   \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m264.0/264.0 kB\u001b[0m \u001b[31m9.9 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hInstalling collected packages: anthropic, langchain-anthropic\n",
            "Successfully installed anthropic-0.51.0 langchain-anthropic-0.3.13\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 1000
        },
        "id": "bIsKAVtvgvFL",
        "outputId": "54cf83b7-25d4-4724-bd98-99fae1f3b194"
      },
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Collecting langchain-google-genai\n",
            "  Downloading langchain_google_genai-2.1.4-py3-none-any.whl.metadata (5.2 kB)\n",
            "Collecting filetype<2.0.0,>=1.2.0 (from langchain-google-genai)\n",
            "  Downloading filetype-1.2.0-py2.py3-none-any.whl.metadata (6.5 kB)\n",
            "Collecting google-ai-generativelanguage<0.7.0,>=0.6.18 (from langchain-google-genai)\n",
            "  Downloading google_ai_generativelanguage-0.6.18-py3-none-any.whl.metadata (9.8 kB)\n",
            "Requirement already satisfied: langchain-core<0.4.0,>=0.3.52 in /usr/local/lib/python3.11/dist-packages (from langchain-google-genai) (0.3.59)\n",
            "Requirement already satisfied: pydantic<3,>=2 in /usr/local/lib/python3.11/dist-packages (from langchain-google-genai) (2.11.4)\n",
            "Requirement already satisfied: google-api-core!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1 in /usr/local/lib/python3.11/dist-packages (from google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (2.24.2)\n",
            "Requirement already satisfied: google-auth!=2.24.0,!=2.25.0,<3.0.0,>=2.14.1 in /usr/local/lib/python3.11/dist-packages (from google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (2.38.0)\n",
            "Requirement already satisfied: proto-plus<2.0.0,>=1.22.3 in /usr/local/lib/python3.11/dist-packages (from google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (1.26.1)\n",
            "Requirement already satisfied: protobuf!=4.21.0,!=4.21.1,!=4.21.2,!=4.21.3,!=4.21.4,!=4.21.5,<7.0.0,>=3.20.2 in /usr/local/lib/python3.11/dist-packages (from google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (5.29.4)\n",
            "Requirement already satisfied: langsmith<0.4,>=0.1.125 in /usr/local/lib/python3.11/dist-packages (from langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (0.3.42)\n",
            "Requirement already satisfied: tenacity!=8.4.0,<10.0.0,>=8.1.0 in /usr/local/lib/python3.11/dist-packages (from langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (9.1.2)\n",
            "Requirement already satisfied: jsonpatch<2.0,>=1.33 in /usr/local/lib/python3.11/dist-packages (from langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (1.33)\n",
            "Requirement already satisfied: PyYAML>=5.3 in /usr/local/lib/python3.11/dist-packages (from langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (6.0.2)\n",
            "Requirement already satisfied: packaging<25,>=23.2 in /usr/local/lib/python3.11/dist-packages (from langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (24.2)\n",
            "Requirement already satisfied: typing-extensions>=4.7 in /usr/local/lib/python3.11/dist-packages (from langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (4.13.2)\n",
            "Requirement already satisfied: annotated-types>=0.6.0 in /usr/local/lib/python3.11/dist-packages (from pydantic<3,>=2->langchain-google-genai) (0.7.0)\n",
            "Requirement already satisfied: pydantic-core==2.33.2 in /usr/local/lib/python3.11/dist-packages (from pydantic<3,>=2->langchain-google-genai) (2.33.2)\n",
            "Requirement already satisfied: typing-inspection>=0.4.0 in /usr/local/lib/python3.11/dist-packages (from pydantic<3,>=2->langchain-google-genai) (0.4.0)\n",
            "Requirement already satisfied: googleapis-common-protos<2.0.0,>=1.56.2 in /usr/local/lib/python3.11/dist-packages (from google-api-core!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (1.70.0)\n",
            "Requirement already satisfied: requests<3.0.0,>=2.18.0 in /usr/local/lib/python3.11/dist-packages (from google-api-core!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (2.32.3)\n",
            "Requirement already satisfied: grpcio<2.0dev,>=1.33.2 in /usr/local/lib/python3.11/dist-packages (from google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (1.71.0)\n",
            "Requirement already satisfied: grpcio-status<2.0.dev0,>=1.33.2 in /usr/local/lib/python3.11/dist-packages (from google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (1.71.0)\n",
            "Requirement already satisfied: cachetools<6.0,>=2.0.0 in /usr/local/lib/python3.11/dist-packages (from google-auth!=2.24.0,!=2.25.0,<3.0.0,>=2.14.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (5.5.2)\n",
            "Requirement already satisfied: pyasn1-modules>=0.2.1 in /usr/local/lib/python3.11/dist-packages (from google-auth!=2.24.0,!=2.25.0,<3.0.0,>=2.14.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (0.4.2)\n",
            "Requirement already satisfied: rsa<5,>=3.1.4 in /usr/local/lib/python3.11/dist-packages (from google-auth!=2.24.0,!=2.25.0,<3.0.0,>=2.14.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (4.9.1)\n",
            "Requirement already satisfied: jsonpointer>=1.9 in /usr/local/lib/python3.11/dist-packages (from jsonpatch<2.0,>=1.33->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (3.0.0)\n",
            "Requirement already satisfied: httpx<1,>=0.23.0 in /usr/local/lib/python3.11/dist-packages (from langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (0.28.1)\n",
            "Requirement already satisfied: orjson<4.0.0,>=3.9.14 in /usr/local/lib/python3.11/dist-packages (from langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (3.10.18)\n",
            "Requirement already satisfied: requests-toolbelt<2.0.0,>=1.0.0 in /usr/local/lib/python3.11/dist-packages (from langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (1.0.0)\n",
            "Requirement already satisfied: zstandard<0.24.0,>=0.23.0 in /usr/local/lib/python3.11/dist-packages (from langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (0.23.0)\n",
            "Requirement already satisfied: anyio in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (4.9.0)\n",
            "Requirement already satisfied: certifi in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (2025.4.26)\n",
            "Requirement already satisfied: httpcore==1.* in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (1.0.9)\n",
            "Requirement already satisfied: idna in /usr/local/lib/python3.11/dist-packages (from httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (3.10)\n",
            "Requirement already satisfied: h11>=0.16 in /usr/local/lib/python3.11/dist-packages (from httpcore==1.*->httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (0.16.0)\n",
            "Requirement already satisfied: pyasn1<0.7.0,>=0.6.1 in /usr/local/lib/python3.11/dist-packages (from pyasn1-modules>=0.2.1->google-auth!=2.24.0,!=2.25.0,<3.0.0,>=2.14.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (0.6.1)\n",
            "Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.11/dist-packages (from requests<3.0.0,>=2.18.0->google-api-core!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (3.4.2)\n",
            "Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.11/dist-packages (from requests<3.0.0,>=2.18.0->google-api-core!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0,>=1.34.1->google-ai-generativelanguage<0.7.0,>=0.6.18->langchain-google-genai) (2.4.0)\n",
            "Requirement already satisfied: sniffio>=1.1 in /usr/local/lib/python3.11/dist-packages (from anyio->httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<0.4.0,>=0.3.52->langchain-google-genai) (1.3.1)\n",
            "Downloading langchain_google_genai-2.1.4-py3-none-any.whl (44 kB)\n",
            "\u001b[2K   \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m44.3/44.3 kB\u001b[0m \u001b[31m2.5 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hDownloading filetype-1.2.0-py2.py3-none-any.whl (19 kB)\n",
            "Downloading google_ai_generativelanguage-0.6.18-py3-none-any.whl (1.4 MB)\n",
            "\u001b[2K   \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m1.4/1.4 MB\u001b[0m \u001b[31m30.8 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hInstalling collected packages: filetype, google-ai-generativelanguage, langchain-google-genai\n",
            "  Attempting uninstall: google-ai-generativelanguage\n",
            "    Found existing installation: google-ai-generativelanguage 0.6.15\n",
            "    Uninstalling google-ai-generativelanguage-0.6.15:\n",
            "      Successfully uninstalled google-ai-generativelanguage-0.6.15\n",
            "\u001b[31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.\n",
            "google-generativeai 0.8.5 requires google-ai-generativelanguage==0.6.15, but you have google-ai-generativelanguage 0.6.18 which is incompatible.\u001b[0m\u001b[31m\n",
            "\u001b[0mSuccessfully installed filetype-1.2.0 google-ai-generativelanguage-0.6.18 langchain-google-genai-2.1.4\n"
          ]
        },
        {
          "output_type": "display_data",
          "data": {
            "application/vnd.colab-display-data+json": {
              "pip_warning": {
                "packages": [
                  "google"
                ]
              },
              "id": "239b701d60074b41a6de4105e76ee8ae"
            }
          },
          "metadata": {}
        }
      ],
      "source": [
        "!pip install -U langchain-google-genai"
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "from typing import Annotated\n",
        "\n",
        "from typing_extensions import TypedDict\n",
        "\n",
        "from langgraph.graph import StateGraph, START\n",
        "from langgraph.graph.message import add_messages\n",
        "\n",
        "\n",
        "class State(TypedDict):\n",
        "    # Messages have the type \"list\". The `add_messages` function\n",
        "    # in the annotation defines how this state key should be updated\n",
        "    # (in this case, it appends messages to the list, rather than overwriting them)\n",
        "    messages: Annotated[list, add_messages]\n",
        "\n",
        "\n",
        "graph_builder = StateGraph(State)"
      ],
      "metadata": {
        "id": "qgEZULyDhx6U"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "from typing import Annotated\n",
        "\n",
        "from langchain.chat_models import init_chat_model\n",
        "from typing_extensions import TypedDict\n",
        "\n",
        "from langgraph.graph import StateGraph, START\n",
        "from langgraph.graph.message import add_messages\n",
        "\n",
        "\n",
        "class State(TypedDict):\n",
        "    messages: Annotated[list, add_messages]\n",
        "\n",
        "\n",
        "graph_builder = StateGraph(State)\n",
        "\n",
        "\n",
        "llm = init_chat_model(\"anthropic:claude-3-5-sonnet-latest\")\n",
        "\n",
        "\n",
        "def chatbot(state: State):\n",
        "    return {\"messages\": [llm.invoke(state[\"messages\"])]}\n",
        "\n",
        "\n",
        "# The first argument is the unique node name\n",
        "# The second argument is the function or object that will be called whenever\n",
        "# the node is used.\n",
        "graph_builder.add_node(\"chatbot\", chatbot)\n",
        "graph_builder.add_edge(START, \"chatbot\")\n",
        "graph = graph_builder.compile()"
      ],
      "metadata": {
        "id": "VScj6nopg6DC"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "import os\n",
        "from langchain.chat_models import init_chat_model\n",
        "\n",
        "os.environ[\"GOOGLE_API_KEY\"] =\"AIzaSyAMx7D-t3XrWtmoqGH4QJWVpYX1fm6FclA\"\n",
        "\n",
        "llm = init_chat_model(\"google_genai:gemini-2.0-flash\")"
      ],
      "metadata": {
        "id": "anVsH7irhUNh"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "def chatbot(state: State):\n",
        "    return {\"messages\": [llm.invoke(state[\"messages\"])]}\n",
        "\n",
        "\n",
        "# The first argument is the unique node name\n",
        "# The second argument is the function or object that will be called whenever\n",
        "# the node is used.\n",
        "graph_builder.add_node(\"1\", chatbot)"
      ],
      "metadata": {
        "id": "Excuq9UVhZRT",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "df638420-5a63-47e2-f4fa-a30195b032da"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stderr",
          "text": [
            "WARNING:langgraph.graph.state:Adding a node to a graph that has already been compiled. This will not be reflected in the compiled graph.\n"
          ]
        },
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "<langgraph.graph.state.StateGraph at 0x7aa2e31ec0d0>"
            ]
          },
          "metadata": {},
          "execution_count": 12
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "graph_builder.add_edge(START, \"chatbot\")\n"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "tgw_e9cChdf-",
        "outputId": "a5a99239-12f4-4d9c-ff6a-2a82b8950baa"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stderr",
          "text": [
            "WARNING:langgraph.graph.graph:Adding an edge to a graph that has already been compiled. This will not be reflected in the compiled graph.\n"
          ]
        },
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "<langgraph.graph.state.StateGraph at 0x7aa2e31ec0d0>"
            ]
          },
          "metadata": {},
          "execution_count": 13
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "from IPython.display import Image, display\n",
        "\n",
        "try:\n",
        "    display(Image(graph.get_graph().draw_mermaid_png()))\n",
        "except Exception:\n",
        "    # This requires some extra dependencies and is optional\n",
        "    pass"
      ],
      "metadata": {
        "id": "rtWPXyi9izVa",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 251
        },
        "collapsed": true,
        "outputId": "5a4d800e-170a-4999-df64-c089add97218"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<IPython.core.display.Image object>"
            ]
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "def stream_graph_updates(user_input: str):\n",
        "    for event in graph.stream({\"messages\": [{\"role\": \"user\", \"content\": user_input}]}):\n",
        "        for value in event.values():\n",
        "            print(\"Assistant:\", value[\"messages\"][-1].content)\n",
        "\n",
        "\n",
        "while True:\n",
        "    try:\n",
        "        user_input = input(\"User: \")\n",
        "        if user_input.lower() in [\"quit\", \"exit\", \"q\"]:\n",
        "            print(\"Goodbye!\")\n",
        "            break\n",
        "        stream_graph_updates(user_input)\n",
        "    except:\n",
        "        # fallback if input() is not available\n",
        "        user_input = \"What do you know about LangGraph?\"\n",
        "        print(\"User: \" + user_input)\n",
        "        stream_graph_updates(user_input)\n",
        "        break"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "i5BPJggIBYBs",
        "outputId": "c9f75c7d-8f75-47be-b053-70ee73bd39d7"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "User: captial of India\n",
            "Assistant: The capital of India is **New Delhi**.\n",
            "User: what is ai agent\n",
            "Assistant: An AI agent is essentially a computer program that can perceive its environment, make decisions, and take actions to achieve a specific goal. Think of it as a virtual robot designed to operate autonomously and intelligently.\n",
            "\n",
            "Here's a breakdown of key aspects:\n",
            "\n",
            "**Core Components:**\n",
            "\n",
            "*   **Perception:** The agent needs to be able to sense or perceive its environment. This might involve reading data from sensors (like cameras, microphones, or thermometers), accessing databases, or receiving information from other agents.\n",
            "*   **Reasoning/Decision-Making:** Based on its perception, the agent must be able to reason about the current situation, predict the consequences of different actions, and choose the best action to take. This often involves using algorithms like machine learning, rule-based systems, or planning algorithms.\n",
            "*   **Action/Execution:** Once the agent has decided on an action, it needs to be able to execute that action in its environment. This could involve controlling a physical robot, sending commands to a computer system, or communicating with other agents.\n",
            "*   **Goal:**  The agent has a specific objective or set of objectives it's trying to achieve.  This goal guides its decision-making process.\n",
            "*   **Environment:** The world the agent operates in.  This could be a physical world (like a factory floor), a virtual world (like a video game), or even a digital environment (like the internet).\n",
            "\n",
            "**Key Characteristics:**\n",
            "\n",
            "*   **Autonomy:** AI agents operate with minimal human intervention. They can make decisions and take actions independently, based on their programming and their perception of the environment.\n",
            "*   **Reactivity:** They respond to changes in their environment in a timely manner.\n",
            "*   **Proactiveness:** They exhibit goal-directed behavior and take initiative to achieve their objectives.\n",
            "*   **Learning:**  Many advanced AI agents can learn from their experiences and improve their performance over time. This is often achieved through machine learning techniques.\n",
            "*   **Adaptability:** They can adjust their behavior in response to changing environments or new information.\n",
            "\n",
            "**Examples of AI Agents:**\n",
            "\n",
            "*   **Chatbots:**  Interact with users to answer questions, provide information, or complete tasks.\n",
            "*   **Virtual Assistants (Siri, Alexa, Google Assistant):** Respond to voice commands, set reminders, play music, and perform other tasks.\n",
            "*   **Recommendation Systems (Netflix, Amazon):** Analyze user data to suggest relevant products or content.\n",
            "*   **Self-Driving Cars:** Perceive the environment, make decisions about navigation, and control the vehicle.\n",
            "*   **Robotic Process Automation (RPA) bots:** Automate repetitive tasks in business processes.\n",
            "*   **Game AI:**  Controls non-player characters (NPCs) in video games.\n",
            "*   **Spam Filters:**  Identify and filter out unwanted emails.\n",
            "*   **Stock Trading Bots:**  Analyze market data and execute trades automatically.\n",
            "\n",
            "**Different Types of AI Agents:**\n",
            "\n",
            "AI agents can be classified in different ways, including:\n",
            "\n",
            "*   **Simple Reflex Agents:**  React directly to percepts based on predefined rules.\n",
            "*   **Model-Based Reflex Agents:** Maintain an internal model of the world to make decisions.\n",
            "*   **Goal-Based Agents:**  Have a specific goal in mind and try to achieve it.\n",
            "*   **Utility-Based Agents:**  Try to maximize their utility (a measure of their happiness or satisfaction).\n",
            "*   **Learning Agents:** Can learn from their experiences and improve their performance over time.\n",
            "\n",
            "**In summary, an AI agent is a sophisticated program that combines perception, reasoning, and action to achieve goals in a dynamic environment.  They are becoming increasingly prevalent in various industries and applications, automating tasks, providing personalized experiences, and solving complex problems.**\n",
            "User: q\n",
            "Goodbye!\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [],
      "metadata": {
        "id": "83WFto1lBdBs"
      },
      "execution_count": null,
      "outputs": []
    }
  ]
}
