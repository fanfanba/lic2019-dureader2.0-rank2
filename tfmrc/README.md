## prepare
```bash
nohup python run.py --prepare --create_vocab true --data_type search \
                    --max_a_len 300 --train_answer_len_cut_bins -1 > search_prepare.log 2>&1 &
nohup python run.py --prepare --create_vocab true --data_type zhidao \
                    --max_a_len 400 --train_answer_len_cut_bins -1 > zhidao_prepare.log 2>&1 &
```

## train
```bash
nohup python run.py --train --gpu 1 --data_type search --desc 'pure_v5' --algo RNET \
                    --max_a_len 300 --train_answer_len_cut_bins 6 --batch_size 32 \
                    --rnn_dropout_keep_prob 0.85 --fuse_dropout_keep_prob 0.9 --weight_decay 0.00006 \
                    --epochs 20 --evaluate_cnt_in_one_epoch 2 --random_seed 2019 \
                    > search.train.log 2>&1 &

nohup python run.py --train --gpu 2 --data_type zhidao --desc 'pure_v5' --algo RNET \
                    --max_a_len 400 --train_answer_len_cut_bins 6 --batch_size 32 \
                    --rnn_dropout_keep_prob 0.85 --fuse_dropout_keep_prob 0.9 --weight_decay 0.00006 \
                    --epochs 20 --evaluate_cnt_in_one_epoch 2 --random_seed 2019 \
                    > zhidao.train.log 2>&1 &
```

## evaluate
```bash
nohup python run.py --evaluate --gpu 3 --data_type search --desc 'pure_v5' \
                    --use_para_prior_scores search --max_a_len 300 --batch_size 128 \
                    > search.evaluate.log 2>&1 &

nohup python run.py --evaluate --gpu 0 --data_type zhidao --desc 'pure_v5' \
                    --use_para_prior_scores zhidao --max_a_len 400 --batch_size 128 \
                    > zhidao.evaluate.log 2>&1 &
```

## predict
```bash
nohup python run.py --predict --gpu 2 --data_type search --desc 'pure_v5' \
                    --max_a_len 300 --use_para_prior_scores search --batch_size 128 \
                    > search_predict2.log 2>&1 &

nohup python run.py --predict --gpu 3 --data_type zhidao --desc 'pure_v5' \
                    --max_a_len 400 --use_para_prior_scores zhidao --batch_size 128 \
                    > zhidao_predict.log 2>&1 &
```
