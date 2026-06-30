the GPT-4 model is significantly superior to the Llama models. The untrained
Llama model answered 28% of the questions with at least
one correct answer, but was also able to answer the majority of the questions and provided no answer in 11% of cases.
The llama2_1e_v100 model provides the fewest correct answers (2%) and demonstrates a general loss of basic linguistic skills. In contrast, the llama2_1e_a30 model shows a comparable, albeit slightly poorer, performance to the untrained model.
The llama2_3e_v100 model achieved a significant improvement in performance compared with one epoch, but its overall performance remains consistently worse than that of the untrained model.
The llama2_3e_a30 model, on the other hand, outperformed the untrained model for the first time, answering 43 per cent of the questions with at least one correct answer.
....

The untrained Llama model, with 39% correct answers,
demonstrated clear strength in multi-fact questions and was able to answer 27% of the
oral exam questions. The worst-performing categories were single-fact questions, with 20% correct answers, and questions from the textbook, with 24% correct answers.
Whilst the llama2_1e_v100 model performed poorly overall, answering only 2 questions correctly, the llama2_1e_a30 model performed significantly better, achieving 36% on multi-fact questions and 40% on oral exam questions. By contrast, this model performed significantly worse on single-fact questions, with 5% correct answers, and on written exam questions, with only 12% correct answers. Models trained on 3 epochs show a marked improvement in performance compared to those trained on just one epoch. Both the llama2_
3e_v100 model and the llama2_3e_a30 model improve their performance on the question types and question sources previously identified as weak. In the case of the llama2_3e_a30 model, this means a doubling of the number of correctly answered transfer questions, as well as a doubling of the number of correctly answered questions from the book. This improvement in performance is also reflected in the MacroF1 scores.
Epochs 5 and 10 contain only models trained on Nvidia A30
graphics cards. The performance shown
differs hardly at all from that of the llama2_3e_a30 model and exhibits only minimal fluctuations in performance. However, the distribution of correctly answered questions is shifting towards questions from the book. The models’ performance on the other question sources deteriorates with each passing epoch, whilst their performance on questions from the book continues to improve. This shift suggests overfitting, as the questions from the book are also included in the training data.